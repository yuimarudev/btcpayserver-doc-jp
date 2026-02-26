# PHP を使った Greenfield API の例

**[Greenfield API](https://docs.btcpayserver.org/API/Greenfield/v1/)**（`/docs` としてあなたのインスタンスでも利用可能）を使うと、使いやすい REST API 経由で BTCPay Server を操作できます。

[Swagger file](https://docs.btcpayserver.org/API/Greenfield/v1/swagger.json) を使えば、任意の言語向けクライアントを部分的に生成できる点にも注意してください。

PHP には専用クライアントライブラリがあり、[こちら](https://github.com/btcpayserver/btcpayserver-greenfield-php) で確認できます。Composer でも `composer require btcpayserver/btcpayserver-greenfield-php` でインストールできます。

このガイドでは、eCommerce 用途と BTCPay 管理用途で、PHP ライブラリを使って Greenfield API を利用する例を紹介します。追加の例は [こちら](https://github.com/btcpayserver/btcpayserver-greenfield-php/tree/master/examples) にあります。


## 前提条件

特定ユーザーの代理でストアや API キーを作成するような一部のエンドポイントを除き、Basic Auth は避け、代わりに API キーを使うべきです。API キーには必要最小限の権限だけを付与してください。たとえば請求書作成だけを行うなら、ストア管理権限は不要です。

BTCPay Server UI の `Account` -> `Manage account` -> `API keys` で新しい API キーを作成できます。

以下の eCommerce 例では、API キーに次の権限が必要です。
- View invoices
- Create invoice
- Modify invoices
- Modify stores webhooks
- View your stores
- 未承認の Pull Payment を作成

利用可能な権限の一覧は [API documentation](https://docs.btcpayserver.org/API/Greenfield/v1/#section/Authentication/API_Key)、または各エンドポイントに記載された権限情報を参照してください。

## eCommerce の例

以下の例では、請求書の作成、Webhook の登録、Webhook の処理、請求書の全額返金という流れで、Greenfield API を使った基本的な eCommerce フローを示します。

### 請求書を作成する

[create invoice endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_CreateInvoice) を使って請求書を作成します。これはシンプルな例ですが、注文 ID、購入者メール、カスタムメタデータなど多くの情報を設定できます。ただし、侵害時の情報漏えいを防ぐため、請求書には冗長なデータを保存しないでください。たとえば多くの場合、顧客住所を eCommerce システムと BTCPay 請求書の両方に保存する意味はありません。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$apiKey = 'API_KEY';
$storeId = 'STORE_ID';
$amount = 10;
$currency = 'USD';
$orderId = 'SN21420';

try {
    $client = new \BTCPayServer\Client\Invoice($host, $apiKey);
    var_dump(
        $client->createInvoice(
            $storeId,
            $currency,
            \BTCPayServer\Util\PreciseNumber::parseString($amount),
            $orderId
        )
    );
} catch (\Throwable $e) {
    echo "Error: " . $e->getMessage();
}
```

### Webhook を登録する（任意）

請求書が支払われたときに通知を受けるため、Webhook を登録します。登録には [create webhook endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Webhooks_CreateWebhook) を使用できます。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$apiKey = 'API_KEY';
$storeId = 'STORE_ID';
$url = 'https://example.com/webhook';
$subscribedEvents = null; // Will subscribe to all events.

try {
    $client = new \BTCPayServer\Client\Webhook($host, $apiKey);
    var_dump(
        $client->createWebhook($storeId, $url, $subscribedEvents, null)
    );
} catch (\Throwable $e) {
    echo "Error: " . $e->getMessage();
}
```

この手順は任意です。ストアの `Settings` -> `Webhooks` から BTCPay Server UI 上で手動作成することもできます。

### Webhook を検証して処理する

BTCPay Server の Webhook ペイロードには署名が付いているため、適切なリクエスト検証後であれば内容を信頼できます。`BTCPay-Sig` HTTP ヘッダーとペイロードの検証は、このライブラリが行います。

Webhook 登録時（上記参照）に、PHP サイトのエンドポイントルートを指す `url` を指定しました。たとえば `https://example.com/webhook` です。リクエスト署名に使う `secret` は、上の例の戻り値で取得できます。

eCommerce サイトでは、次のように BTCPay Server Webhook のペイロードを検証して処理できます。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$apiKey = 'API_KEY';
$storeId = 'STORE_ID';
$webhookSecret = 'WEBHOOK_SECRET'; // From previous step

// Get the data sent by BTCPay Server.
$raw_post_data = file_get_contents('php://input');
$payload = json_decode($raw_post_data, false, 512, JSON_THROW_ON_ERROR);

// Get the BTCPay signature header.
// This is needed as some webservers camel-case the headers, some not.
$headers = getallheaders();
foreach ($headers as $key => $value) {
    if (strtolower($key) === 'btcpay-sig') {
        $sig = $value;
    }
}

$webhookClient = new \BTCPayServer\Client\Webhook($host, $apiKey);

// Validate the webhook request.
if (!$webhookClient->isIncomingWebhookRequestValid($raw_post_data, $sig, $secret)) {
    throw new \RuntimeException(
        'Invalid BTCPay Server payment webhook message received - signature did not match.'
    );
}

echo 'Validation OK';

// Your own processing code goes here. E.g. update your internal order id depending on the invoice payment status.

```

### 請求書を全額返金する

[invoice refund endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_Refund) を使うと、請求書の全額（または一部）返金を実行できます。顧客が返金を請求できるリンクが返されます。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$apiKey = 'API_KEY';
$storeId = 'STORE_ID';
$invoiceId = 'EXISTING_INVOICE_ID';

try {
    $client = new \BTCPayServer\Client\Invoice($host, $apiKey);

    $refund = $client->refundInvoice(
        $storeId,
        $invoiceId
    );

    echo $refund->getViewLink();
} catch (\Throwable $e) {
    echo "Error: " . $e->getMessage();
}
```


## BTCPay Server 管理の例

ここでは、あなたがアンバサダーとしてユーザー向けに BTCPay Server をホストしている想定です。ユーザー管理は自分のシステムで行い、BTCPay Server ログイン用のメールとパスワードを設定してユーザーを作成します。その後、同じ認証情報を使ってユーザーの代理でストアと API キーを作成します。

### 新しいユーザーを作成する

新規ユーザーは [this endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Users_CreateUser) で作成できます。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$adminApiKey = 'ADMIN_API_KEY';
$email = 'satoshi.nakamoto@example.com';
$password = 'SuperSecurePasswordsShouldBeQuiteLong123';
$isAdministrator = false;

try {
    $client = new \BTCPayServer\Client\User($host, $adminApiKey);
    var_dump(
        $client->createUser($email, $password, $isAdministrator)
    );
} catch (\Throwable $e) {
    echo "Error: " . $e->getMessage();
}
```

### 新しい API キーを作成する（ユーザー向け）

Basic 認証でも Greenfield API にアクセスできますが、認証情報のスコープを制限するため API キーの使用が推奨されます。

たとえば [create a new store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_CreateStore) を行うには、API キーに `btcpay.store.canmodifystoresettings` 権限が必要です。注意: 権限を1つも渡さない場合、API キーは無制限アクセスになります。

前述の通り、これはインスタンスの BTCPay Server UI からも実行できますが、ここでは [this endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/ApiKeys_CreateUserApiKey) を使って API 経由で行います。管理者 API キーを使い、新規ユーザー向け API キーを作成します。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$userEmail = 'satoshi.nakamoto@example.com';
$adminApiKey = 'ADMIN_API_KEY';

try {
    $client = new \BTCPayServer\Client\ApiKey($host, $adminApiKey);
    $generatedApiKey = $client->createApiKeyForUser($userEmail, 'api generated', ['btcpay.store.canmodifystoresettings']);
} catch (\Throwable $e) {
    echo "Error: " . $e->getMessage();
}

echo $generatedApiKey->getData()['apiKey'];
```

### 新しいストアを作成する

次に、ユーザーの API キーを使って [create a new store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_CreateStore) を行います。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$userApiKey = 'USER_API_KEY'; // From previous step

try {
  $client = new \BTCPayServer\Client\Store($host, $userApiKey);
  var_dump($client->createStore('my new store'));
} catch (\Throwable $e) {
  echo "Error: " . $e->getMessage();
}
```

### ストア情報を読み取る

新しい API キーを使って [read store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_GetStore) 情報を取得できます。

```PHP
require __DIR__ . './vendor/autoload.php';

$host = 'https://mainnet.demo.btcpayserver.org';
$userApiKey = 'USER_API_KEY'; // From previous step
$storeId = 'STORE_ID'; // From previous step

try {
  $client = new \BTCPayServer\Client\Store($host, $userApiKey);
  var_dump($client->getStore($storeId));
} catch (\Throwable $e) {
  echo "Error: " . $e->getMessage();
}
```

さらに多くの例は [こちら](https://github.com/btcpayserver/btcpayserver-greenfield-php/tree/master/examples) で確認できます。
