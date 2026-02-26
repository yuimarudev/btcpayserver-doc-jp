# Node.js (JavaScript) での Greenfield API 例


**[Greenfield API](https://docs.btcpayserver.org/API/Greenfield/v1/)**（インスタンス上の `/docs` でも利用可能）を使うと、使いやすい REST API 経由で BTCPay Server を操作できます。

[Swagger file](https://docs.btcpayserver.org/API/Greenfield/v1/swagger.json) を使うことで、任意の言語向けクライアントを部分的に自動生成できます。

このガイドでは、Node.js/JavaScript での使い方を紹介します。

## 前提条件

特定ユーザーの代理でストアや API キーを作るような一部エンドポイントを除き、Basic Auth は避けて API キーを使うべきです。API キーには必要最小限の権限だけを与えてください。たとえば請求書の作成だけが目的なら、ストア管理権限は不要です。

新しい API キーは、BTCPay Server UI の `Account` -> `Manage account` -> `API keys` から作成できます。

以下の eCommerce 例では、API キーに次の権限が必要です。
- View invoices
- Create invoice
- Modify invoices
- Modify stores webhooks
- View your stores
- 未承認の Pull Payment を作成

利用可能な権限の概要は [API documentation](https://docs.btcpayserver.org/API/Greenfield/v1/#section/Authentication/API_Key) または各エンドポイントの権限説明を参照してください。

## eCommerce 例

以下の例では、Greenfield API を使って、請求書作成、Webhook 登録、Webhook 処理、請求書の全額返金までを含む基本的な eCommerce フローを構築する方法を示します。

### 請求書を作成する

[create invoice endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_CreateInvoice) を使って請求書を作成します。これはシンプルな例ですが、注文 ID、購入者メール、カスタムメタデータなど、さらに多くのデータを設定できます。ただし、侵害時の情報漏えいを防ぐため、請求書に冗長データを保存しないでください。たとえば多くの場合、顧客住所を eCommerce システムと BTCPay 請求書の両方に保存する意味はありません。

```JS
const btcpayServerUrl = 'https://mainnet.demo.btcpayserver.org'
const storeId = 'YOUR_STORE_ID'
const apiKey = 'YOUR_API_KEY'
const amount = 10
const currency = 'USD'

const apiEndpoint = `/api/v1/stores/${storeId}/invoices`

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + apiKey
}
const payload = {
  amount: amount,
  currency: currency
}
fetch(btcpayServerUrl + apiEndpoint, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    console.log(data)
  })
```

### Webhook を登録する（任意）

請求書の支払いを通知するための Webhook を登録しましょう。Webhook の登録には [create webhook endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Webhooks_CreateWebhook) を使えます。

```JS
const btcpayServerUrl = 'https://mainnet.demo.btcpayserver.org'
const storeId = 'YOUR_STORE_ID'
const apiKey = 'YOUR_API_KEY'

const apiEndpoint = `/api/v1/stores/${storeId}/webhooks`

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + apiKey
}

const payload = {
  url: 'https://example.com/your-webhook-endpoint'
}
fetch(btcpayServerUrl + apiEndpoint, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    console.log(data)
  })
```

この手順は任意です。BTCPay Server UI でストアの `Settings` -> `Webhooks` から手動で Webhook を作成することもできます。

### Webhook を検証して処理する

Node.js Express の Web アプリで、BTCPay Server からの Webhook リクエストを受信できます。

まず、Node.js アプリで POST リクエストを受け取るルートが必要です。
Express サーバーの構成次第ですが、次のようになります。

```JS
app.post('/your-webhook-endpoint', (req, res) => {
  // Do stuff here
})
```

重要なのは、Webhook が `BTCPAY-SIG` という HTTP ヘッダーを送ることです。これは、Webhook 登録時に取得した `secret` を使って署名されたリクエストです。この `secret` と Webhook から受け取る raw payload（バイト列）を使ってハッシュを計算し、`BTCPAY-SIG` と比較できます。そのため、リクエスト本文の raw body をパースするミドルウェア `body-parser` が必要です。ハッシュ比較には Node.js 組み込みの `crypto` モジュールも必要です。
```JS
const bodyParser = require('body-parser')
const crypto = require('crypto')
```

リクエストの raw body は次のようにパースできます。

```JS
app.use(
  bodyParser.json({
    verify: (req, res, buf) => {
      req.rawBody = buf
    }
  })
)
```
これにより `req.rawBody` に正しい内容が格納され、`req.rawBody` のハッシュ値と `BTCPAY-SIG` ヘッダー値を比較できるようになります。

ただし TypeScript を使っている場合、次のエラーが出ることがあります。

```
Property 'rawBody' does not exist on type 'Request'.
```

一時的な回避策として、`as` キーワードでコンパイラに `req` オブジェクトを別型として扱わせ、`rawBody` にアクセスします。次はこの回避策で `rawBody` を取得する例です（上記ミドルウェアコードは不要）。

```JS
import { Request, Response } from "express-serve-static-core"
import { https } from "firebase-functions";

type FirebaseRequest = https.Request

const myFunc = async (req: Request, res: Response) => {
  const rawBody = (req as FirebaseRequest).rawBody;
}
```
TypeScript でこの方法を使う場合、以降のセクションでは `req.rawBody` を `rawBody` に置き換えてください。

ルーターでまとめると次のようになります（`webhookSecret` は、Webhook 登録時に取得した `secret` に置き換えてください）。

```JS
app.post('/your-webhook-endpoint', (req, res) => {
  const sigHashAlg = 'sha256'
  const sigHeaderName = 'BTCPAY-SIG'
  const webhookSecret = 'SECRET_FROM_REGISTERING_WEBHOOK' // see previous step
  if (!req.rawBody) {
    res.status(500).send('Request body empty')
  }
  const checksum = Buffer.from(req.get(sigHeaderName) || '', 'utf8')
  const hmac = crypto.createHmac(sigHashAlg, webhookSecret)
  const digest = Buffer.from(
    sigHashAlg + '=' + hmac.update(req.rawBody).digest('hex'),
    'utf8'
  )

  if (
    checksum.length !== digest.length ||
    !crypto.timingSafeEqual(digest, checksum)
  ) {
    console.log(`Request body digest (${digest}) did not match ${sigHeaderName} (${checksum})`)
    res.status(500).send(`Request body digest (${digest}) did not match ${sigHeaderName} (${checksum})`)
  } else {

    // Your own processing code goes here. E.g. update your internal order id depending on the invoice payment status.

    res.status(200).send('Success: request body was signed')
  }
})
```

### 請求書の全額返金を実行する

[invoice refund endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_Refund) を使うと、請求書の全額（または部分）返金を実行できます。顧客が返金を受け取るためのリンクが返されます。

```JS
const btcpayServerUrl = 'https://mainnet.demo.btcpayserver.org'
const storeId = 'YOUR_STORE_ID'
const apiKey = 'YOUR_API_KEY'
const invoiceId = 'EXISTING_INVOICE_ID'

const apiEndpoint = `/api/v1/stores/${storeId}/invoices/${invoiceId}/refund`

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + apiKey
}

const payload = {
  refundVariant: 'CurrentRate',
  paymentMethod: 'BTC'
}

fetch(btcpayServerUrl + apiEndpoint, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    console.log(data)
    res.send(data)
  })
```

## BTCPay Server 管理の例

ここでは、あなたがアンバサダーとしてユーザー向けに BTCPay Server をホストしている想定です。自分のシステムでユーザー管理を行い、BTCPay Server ログイン用のユーザーを作成し、メールとパスワードを設定します。その後、同じ認証情報を使って、そのユーザーの代理でストアと API キーを作成します。

### 新しいユーザーを作成する

新規ユーザー作成は [this endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Users_CreateUser) で実行できます。

```JS
const btcpayServerUrl = 'https://mainnet.demo.btcpayserver.org'
const adminApiKey = 'YOUR_ADMIN_API_KEY'

const apiEndpoint = '/api/v1/users'

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + adminApiKey
}

const payload = {
  email: 'satoshi.nakamoto@example.com',
  password: 'SuperSecurePasswordsShouldBeQuiteLong123',
  isAdministrator: false
}

fetch(btcpayServerUrl + apiEndpoint, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    console.log(data)
    res.send(data)
  })
```

### 新しい API キーを作成する（ユーザー向け）

Greenfield API へのアクセスに Basic 認証も利用できますが、認証情報のスコープを限定するため API キーの利用が推奨されます。

例として、[create a new store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_CreateStore) するには API キーに `btcpay.store.canmodifystoresettings` 権限が必要です。注意: 権限を1つも渡さない場合、その API キーは無制限アクセスになります。

前述のとおり、これはインスタンスの BTCPay Server UI からも実行できますが、ここでは [this endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/ApiKeys_CreateUserApiKey) を使って、admin API キーで新規ユーザー用 API キーを作成します。

```js
const btcpayServerUrl = 'https://mainnet.demo.btcpayserver.org'
const adminApiKey = 'YOUR_ADMIN_API_KEY'
const email = 'satoshi.nakamoto@example.com'

const apiEndpoint = `/api/v1/users/${email}/api-keys`

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + adminApiKey
}

const payload = {
  label: 'Satoshi Nakamoto API Key',
  permissions: ['btcpay.store.canmodifystoresettings']
}

fetch(btcpayServerUrl + apiEndpoint, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    console.log(data) // returns apiKey
    res.send(data)
  })
```

### 新しいストアを作成する

次に、API キーを使って [create a new store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_CreateStore) できます。

```JS
const btcpayserverUrl = 'https://mainnet.demo.btcpayserver.org'
const userApiKey = 'USER_API_KEY' // From previous step

const apiEndpoint = '/api/v1/stores'

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + userApiKey
}
const payload = {
  name: 'Satoshi Store'
}

fetch(btcpayServerUrl + apiEndpoint, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    console.log(data)
    res.send(data)
  })
```

### ストア情報を取得する

新しい API キーで [read store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_GetStore) 情報を取得できます。

```JS
const btcpayServerUrl = 'https://mainnet.demo.btcpayserver.org'
const userApiKey = 'USER_API_KEY' // From previous step
const storeId = 'STORE_ID' // From previous step

const apiEndpoint = `/api/v1/stores/${storeId}`

const headers = {
  'Content-Type': 'application/json',
  Authorization: 'token ' + userApiKey
}

fetch(btcpayServerUrl + apiEndpoint, {
  method: 'GET',
  headers: headers
})
  .then(response => response.json())
  .then(data => {
    console.log(data)
    res.send(data)
  })
```
