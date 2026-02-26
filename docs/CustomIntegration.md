# カスタム連携のために BTCPay API を使う

BTCPay Server は、連携のために 2 つの API を提供しています。

- [GreenField API](/Development/GreenFieldExample.md) - BTCPay Server をヘッドレスで利用できることを目的とした RESTful API です。Bitpay 連携のコードを流用しないプロジェクトには、こちらの API を推奨します。
- Bitpay Invoice API - BTCPay は、請求書の作成・管理について Bitpay と同じ API を実装しています。

**BitPay から BTCPay への移行**は、通常 URL を変更するだけで済みます。

Bitpay が 1 商人につき 1 アカウントのみ許可しているのに対し、BTCPay では 1 人のユーザーが複数ストアを管理できます。

## 公式クライアントライブラリ

BTCPay は [C#](https://github.com/MetacoSA/NBitpayClient)、[Python](https://github.com/btcpayserver/btcpay-python)、[NodeJS](https://github.com/btcpayserver/node-btcpay) の公式クライアントライブラリを提供しています。

さらに、Bitpay クライアントの [PHP](https://github.com/btcpayserver/btcpayserver-php-client) および [Ruby](https://github.com/bitpay/ruby-client) のフォークリポジトリもあります。

## API へ手動でアクセスする

上記ライブラリを使わない場合、REST API には手動でアクセスできます。

認証メカニズムには `BitId` を使用します。

`BitId` では、API の `client`（e コマースプラグインなど）が秘密鍵を生成し、`server`（BTCPay）に公開鍵を通知します。

クライアントから API に送るすべてのリクエストは、クライアントの `private key` で署名されます。

BTCPay に `public key` を通知するプロセスを `pairing` と呼びます。

## ペアリングプロセス

まず新しいストアを作成する必要があります。

1. ログイン
2. Stores メニューへ移動
3. `Create a new store` をクリック
4. ストアの表示名を入力して確定

`pairing` には、クライアント側ペアリングとサーバー側ペアリングの 2 つの方法があります。

### クライアント側ペアリング

クライアント側ペアリングでは、`client` が自身の `public key` から URL を生成し、利用者がその URL を開いてペアリングを承認します。

通常、URL は `https://btcpay.example.com/api-access-request?pairingCode=<pairingcode_goes_here>` のような形式です。

この方法の実装方法は [こちらのリンク](https://support.bitpay.com/hc/en-us/articles/115003001183-How-do-I-pair-my-client-and-create-a-token-) を参照してください。

### サーバー側ペアリング

もう一つの方法は、何らかの bitcoin ライブラリで秘密鍵を生成したうえで、次を行うことです。

1. ストア設定へ移動
2. `Access tokens` をクリック
3. `Create new Token` をクリック
4. merchant facade を選択して公開鍵を入力
5. request pairing をクリック
6. Approve をクリック

## 注記

**BTCPay Server は Bitpay 互換の API を備えている**ため、e コマースアプリケーションを **Bitpay から BTCPay に切り替える**作業は最小限で済みます。

完全な API ドキュメントは [Bitpay のウェブサイト](https://bitpay.com/api#resource-Invoices) で確認できます。

違いは 1 点だけです。Bitpay は 1 商人につき 1 アカウントのみ許可しますが、BTCPay は 1 人のユーザーが複数ストアを管理できます。

## モーダルチェックアウト

ポップアップモーダルの体験を生成するには次を実行します。

1. HTML ページに btcpay.js スクリプトを含める

```html
<script src="https://your.btcpay.url/modal/btcpay.js"></script>
```

2. Invoice API を呼び出して請求書を生成する（コード例）。これは認証トークンを含むため、フロントエンドへ公開すべきでないバックエンドのサンプルコードです。

```js
const axiosClient = axios.create({
  baseURL: BTCPAY_URL,
  timeout: 5000,
  responseType: 'json',
  headers: {
    'Content-Type': 'application/json',
    Authorization: BTCPAY_AUTH
  }
})

const invoiceCreation = {
  price: 12345,
  currency: 'USD',
  orderId: 'something',
  itemDesc: 'item description',
  notificationUrl: 'https://webhook.after.checkout.com/goeshere',
  redirectURL: 'https://go.here.after.checkout.com'
}

const response = await axiosClient.post('/invoices', invoiceCreation)
const invoiceId = response.data.data.id
```

3. invoiceId を使ってモーダルを表示する

```js
window.btcpay.showInvoice(invoiceId)
```

4. 請求書が支払われたときにページ状態を更新したり、モーダル表示前後で何らかの状態を記録したりしたい場面はよくあります。次のようにイベントリスナーを追加できます。

```js
window.btcpay.onModalWillEnter(yourCallbackFunction)
window.btcpay.onModalWillLeave(yourCallbackFunction)
window.btcpay.onModalReceiveMessage(yourCallbackFunction) // available from v1.0.5.6
```

`onModalReceiveMessage` は、請求書 UI に新しいステータスが BTCPay Server から push されたときにコールバックを呼び出します。データ形式は `{invoiceId: "x", status: "y" }` です。
