
# eコマース統合ガイド

このドキュメントは、BTCPay Server を eコマース、ERP、POS（Point of Sale）やその他システムに決済ソリューションとして統合するためのベストプラクティスとガイドを提供することを目的としています。BTCPay 上のストアやユーザー管理も含む、より広範な統合を行いたい場合でも、このガイドは出発点として有用です。BTCPay Server をヘッドレスで管理する方法の例は、ドキュメント内の [cURL](https://docs.btcpayserver.org/Development/GreenFieldExample/)、[Node.js](https://docs.btcpayserver.org/Development/GreenFieldExample-NodeJS/)、[PHP](https://docs.btcpayserver.org/Development/GreenfieldExample-PHP/) を参照してください。


## 目次
[[toc]]

## 一般情報

狙いは、ユーザーにとってセットアップがスムーズで、その顧客にとってもチェックアウト体験がシームレスであることです。以下で説明する自動セットアップウィザードを提供すれば、多くのドキュメントを書く手間を減らし、できるだけ簡単な導入体験を実現できます。

## Greenfield API ドキュメント

いわゆる Greenfield API のドキュメントは [こちら](https://docs.btcpayserver.org/API/Greenfield/v1/)（またはあなたの BTCPay Server の `/docs` パス）で確認できます。クイックスタートとして、最も基本的な eコマース統合エンドポイントの例を [cURL](https://docs.btcpayserver.org/Development/GreenFieldExample/)、[Node.js](https://docs.btcpayserver.org/Development/GreenFieldExample-NodeJS/)、[PHP](https://docs.btcpayserver.org/Development/GreenfieldExample-PHP/) 向けに用意しています。このドキュメントでもそれらの例を参照します。

以下の例では、デモサーバー [https://mainnet.demo.btcpayserver.org](https://mainnet.demo.btcpayserver.org) を使用しています。実際の統合では、これはユーザーが指定する BTCPay Server インスタンス URL になります。

## 認証

認証は Basic-Auth または API キーで行えます。Basic-Auth は常にユーザーのフルアクセス権限を持つため推奨されません。API キーであればストア単位で細かい権限を付与でき、万が一 API キーが漏えいしても被害を最小化できます。

**重要**: Authorization ヘッダーは次の形式です。

`Authorization: token API_KEY`
（API_KEY を実際の API キーに置き換えてください）

cURL を使った例:
```bash
curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token API_KEY" \
     -X GET \
     "https://mainnet.demo.btcpayserver.org/api/v1/stores/STORE_ID"
```

### API キー

API キーは [authorization endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#tag/Authorization) から生成できます（詳細は以下の [自動セットアップ](#automatic-setup) を参照）。また、テスト目的であれば [手動セットアップ](#manual-setup) の説明どおり手動で作成することもできます。

### 権限

一般的な eコマース統合で、決済処理と返金を行うための出発点として必要な権限は次のとおりです。

`btcpay.store.canviewinvoices` \
`btcpay.store.cancreateinvoice` \
`btcpay.store.canmodifyinvoices` \
`btcpay.store.webhooks.canmodifywebhooks` \
`btcpay.store.canviewstoresettings` \
`btcpay.store.cancreatenonapprovedpullpayments`

これにより、請求書作成、返金、Webhook のプログラム登録に必要な最小限の権限を付与できます。API キーは特定ストアに限定してください。限定しない場合、ユーザーが持つすべてのストアで API キーが機能してしまいます。


## セットアップのベストプラクティス

<a id="automatic-setup"></a>
### 自動セットアップ

![アプリ認可、API キー生成](../img/ecommerce-integration-guide/btcpay-authorize-app-api-key.png)

ユーザーにとって eコマースシステムと BTCPay Server の接続フローをできるだけスムーズで簡単にするために、セットアップウィザードへ案内できます。理想的には、ユーザーは自分の BTCPay Server インスタンス URL（例: [https://mainnet.demo.btcpayserver.org](https://mainnet.demo.btcpayserver.org)）を入力し、"_BTCPay Server に接続_" ボタンを押すだけで次の処理が始まる形です。

* ユーザーが BTCPay Server の認可ページにリダイレクトされる
* ユーザーが BTCPay Server アカウントでログインする（未ログインの場合）
* ユーザーが対象ストアを選択する（複数ストアを持つ場合）
* ユーザーが BTCPay Server アカウント内で API キーを識別するためのラベルを入力できる（任意）
* 必要な権限（上のスクリーンショット参照）があらかじめ入力済みになっている
* ユーザーが "Authorize app" をクリックすると、BTCPay が権限付きかつ単一ストアに紐づいた API キーを生成し、あなたの eコマースシステムへ返す
* 返却されたペイロード（API キーと store id を含む）をあなたの側で処理する
* その API キーを使って、ユーザーのストアにあなたの eコマースシステムの Webhook エンドポイントを登録する
* そのエンドポイントは Webhook の "secret" を返すため、後で受信する請求書ステータス変更イベントの検証に使えるよう保存しておく
* 以上の情報を eコマースシステムに保存してセットアップ完了

#### [Authorization endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/ApiKeys_Authorize) のリクエスト例

**重要**: `selectiveStores` は `true` に設定する必要があります。これにより、API キーの権限対象となるストアをユーザーが明示的に 1 つ選択するようになります。そうしないと複数ストアを選択できてしまいます。

```bash
curl -s \
      -H "Content-Type: application/json" \
      -X GET \
      "https://mainnet.demo.btcpayserver.org/api-keys/authorize?permissions=btcpay.store.canviewinvoices&permissions=btcpay.store.cancreateinvoice&permissions=btcpay.store.canmodifyinvoices&permissions=btcpay.store.webhooks.canmodifywebhooks&permissions=btcpay.store.canviewstoresettings&permissions=btcpay.store.cancreatenonapprovedpullpayments&strict=true&selectiveStores=true&applicationName=YourAppName&redirect=https://example.com/your-callback-url"
```

<a id="manual-setup"></a>
### 手動セットアップ

あなた自身またはユーザーは、BTCPay Server の UI で API キーを手動作成することもできます。ただし、自動セットアップの方がユーザーフレンドリーでミスも起きにくいです。手動セットアップの手順は次のとおりです。

* ユーザーが BTCPay Server インスタンスにログインする
* ユーザーが "_Account_" -> "_Manage Account_" -> "_API keys_" をクリックする
* 上記権限を持ち、特定ストアに限定した API キーを作成する
* API キーと store id をあなたの入力フォームにコピーする
* ストアで Webhook を作成する: "_Settings_" -> "_Webhooks_"; `secret` を設定フォームにコピーする

## チェックアウトフロー

### チェックアウトフロー概要

**_凡例_**:

**FE**: フロントエンド（あなたの Web サイト/既存システム） \
**BE**: バックエンド（あなたの Web サイト/既存システム） \
**BTCPay**: あなた自身または第三者がホストする BTCPay Server インスタンス

![BTCPay チェックアウトフローチャート](../img/ecommerce-integration-guide/btcpay-checkout-flow-chart.png)

1. FE: チェックアウト時にユーザーが支払い方法を選択する（この例では Bitcoin（via BTCPay））
2. FE/BE: チェックアウトデータが BE に渡され、BE が BTCPay で [請求書を作成](#creating-an-invoice) する。BE は後で照合できるよう、請求書 ID を内部の注文 ID またはチェックアウト ID とあわせて保存する。
3. FE オプション 1: 顧客を BTCPay の請求書ページへリダイレクトし、QR コードと支払いオプションを表示する  \
   FE オプション 2: 顧客にモーダルオーバーレイ内で BTCPay チェックアウトページを表示する
4. 顧客が請求書を支払う
5. BTCPay: 支払いがメモリプールで確認されると（かつ請求書が全額支払い済みになると）、`InvoiceProcessing` イベントで Webhook を送信する。BE は支払いステータスを `pending`、`approved`、`on-hold` などに更新する必要がある。
6. 顧客は自動またはボタンクリックで注文確認ページへリダイレクトされる
7. BTCPay: 設定された承認数（デフォルトは 1 承認）に応じて `InvoiceSettled` イベントの Webhook を送信する。BE は注文ステータスを `paid`、`settled` などに更新する。

注: 5. と 7. の Webhook は、オンチェーン Bitcoin 取引では平均 10 分程度の時間差で発火します（次のブロック生成タイミングに依存）。Lightning Network（オフチェーン）取引では、これら 2 つのイベントは連続して即時に発火し、支払いは確定します。


### 支払いゲートウェイとして BTCPay（Bitcoin / Lightning Network）を表示する

これはあなたのチェックアウトフロー実装に大きく依存します。多くの eコマースストアでは支払いステップがあり、そこで複数の支払い方法を表示します。通常は "Bitcoin / Lightning Network" を表示します。デフォルトでは、BTCPay Server 側で設定されたすべての支払い方法が、QR コードを表示する請求書ページで利用可能になります。より高度なユースケースでは、対応支払い方法を個別に表示し、請求書作成時にそれらを指定することもできます。これにより、たとえばオンチェーン決済より Lightning Network 決済に割引を適用することが可能です。

<a id="creating-an-invoice"></a>
### 請求書の作成

顧客が FE で BTCPay（Bitcoin / Lightning Network）を支払い方法として選択したら、BE は create invoice endpoint を使って請求書を作成する必要があります。これにより請求書 ID を含む請求書オブジェクトが返されます。この請求書 ID は Webhook で使われるため、注文とあわせて保存してください。返却される請求書オブジェクトには `checkoutLink` プロパティも含まれており、顧客を支払いページにリダイレクトするために使えます（詳細は下記）。

これは [create invoice endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_CreateInvoice) を使って実行します。例は [cURL](https://docs.btcpayserver.org/Development/GreenFieldExample/#create-an-invoice)、[Node.Js](https://docs.btcpayserver.org/Development/GreenFieldExample-NodeJS/)、[PHP](https://docs.btcpayserver.org/Development/GreenfieldExample-PHP/#create-an-invoice) を参照してください。

請求書 ID は内部の注文またはチェックアウトオブジェクトと必ず一緒に保存することが重要です。これにより、[Webhook イベント](#invoice-webhook-events) を処理するときに対応する内部注文を特定できます。

### リダイレクト

請求書オブジェクトには `checkoutLink` プロパティが含まれており、これはその請求書専用の BTCPay チェックアウトページ URL です。顧客をそのページにリダイレクトして支払いを行ってもらえます。

支払い成功後、顧客はストアに戻るか、`checkout` オブジェクトの `redirectURL` で指定した URL にリダイレクトされます。これは請求書作成時に設定できます。

### モーダル請求書ページ（高度・任意）

顧客を外部の BTCPay 請求書チェックアウトページへリダイレクトする代わりに、ストアのチェックアウト画面上にモーダル/オーバーレイとして直接表示することもできます。この場合は BTCPay インスタンスから `btcpay.js` を読み込み、支払いイベントを監視する JavaScript をフロントエンドに追加する必要があります。

BTCPay インスタンスから `btcpay.js` スクリプトを読み込みます:
`https://mainnet.demo.btcpayserver.org/modal/btcpay.js`

以下は支払いイベントを監視する簡略例です。完全な例は WooCommerce プラグインの [こちら](https://github.com/btcpayserver/woocommerce-greenfield-plugin/blob/master/resources/js/frontend/modalCheckout.js) を参照してください。

```js
let invoice_paid = false;
window.btcpay.onModalReceiveMessage(function (event) {
  if (isObject(event.data)) {
    if (event.data.status) {
      switch (event.data.status) {
        case 'complete':
        case 'paid':
          invoice_paid = true;
          window.location = data.orderCompleteLink;
          break;
        case 'expired':
          window.btcpay.hideFrame();
          // Show some error to the user.
          break;
      }
    }
  } else { // handle event.data "loaded" "closed"
    if (event.data === 'close') {
      if (invoice_paid === true) {
        window.location = data.orderCompleteLink;
      }
      // Show some error to the user, user closed the modal by clicking the X.
    }
  }
});
const isObject = obj => {
  return Object.prototype.toString.call(obj) === '[object Object]'
}
```

### Webhook の検証と処理

<a id="invoice-webhook-events"></a>
#### 請求書 Webhook イベント

利用可能な請求書 Webhook イベントは次のとおりです。詳細は API ドキュメントへのリンクを参照してください。
- **InvoiceCreated**: 請求書が作成された
- **InvoiceReceivedPayment**: 支払いがメモリプールで確認された（未承認）
- **InvoicePaymentSettled**: 支払いがブロックチェーンで承認された
- **InvoiceProcessing**: 請求書の全額支払いがメモリプールで確認された（未承認）。BE は注文ステータスを `pending` または `approved` に更新する必要がある
- **InvoiceExpired**: 制限時間内に支払いがメモリプールで確認されなかった。BE は注文ステータスを `expired` に更新する必要がある
- **InvoiceSettled**: 支払いがブロックチェーンで承認された。BE は注文ステータスを `paid` または `settled` に更新する必要がある
- **InvoiceInvalid**: 支払いが無効。BE は注文ステータスを `invalid` に更新する必要がある

特に重要なイベントは次のとおりです。
- InvoiceProcessing
- InvoiceExpired
- InvoiceSettled
- InvoiceInvalid

ただし、期限切れ後の支払いなどを処理するために、次のイベントも必要です。
- InvoiceReceivedPayment
- InvoicePaymentSettled

#### 請求書の状態

通常、BE 側の注文ステータス設定には Webhook イベントだけで十分で、請求書状態を確認する追加の GET リクエストは不要です。ただし、場合によっては追加確認が必要です。その場合は [get invoice endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_GetInvoice) を使って実際の状態を確認できます。

Webhook イベントと同様に、部分支払い・過払いなどのエッジケースを考慮する必要があります。返却される請求書オブジェクトには、現在の状態を明確に示す重要なプロパティが 2 つあります。
- **status**: `Expired` `Invalid` `New` `Processing` `Settled`
- **additionalStatus**: `Invalid` `Marked` `None` `PaidLate` `PaidOver` `PaidPartial`

請求書ステータスを正しく把握するには、常に両方のプロパティを確認する必要があります。請求書が期限切れでも、期限後に部分支払いまたは全額支払いされている可能性があります。

#### Webhook 署名の検証

Webhook イベントを受信したら、そのイベントが本当にあなたの BTCPay Server インスタンスから送られたものか確認するため、署名を検証してください。署名はイベントペイロードと Webhook secret から生成した HMAC-SHA256 ハッシュです。secret はストアに Webhook を登録したときに返されます（前述）。BTCPay Server UI の "_Settings_" -> "_Webhooks_" でも確認できます。

[Node.Js](https://docs.btcpayserver.org/Development/GreenFieldExample-NodeJS/#validate-and-process-webhooks) と [PHP](https://docs.btcpayserver.org/Development/GreenfieldExample-PHP/#validate-and-process-webhooks) の例はドキュメントを参照してください。

#### Webhook イベントの処理

各 Webhook ペイロードには `invoiceId` プロパティがあり、これはそのイベントが関連する請求書の ID です。請求書 ID を注文と一緒に保存しているため、対応する注文を特定して状態を更新できます。

多くの場合、`InvoiceCreated` イベントは無視して問題ありません。チェックアウト処理中に請求書を作成しており、すでに請求書 ID を注文と一緒に保存しているためです。ただし、期限切れ請求書への部分支払い・過払いといったエッジケースは存在します。これらの処理例は WooCommerce プラグインの PHP コード [こちら](https://github.com/btcpayserver/woocommerce-greenfield-plugin/blob/master/src/Gateway/AbstractGateway.php#L504) を確認してください。

:::tip
テスト目的で、請求書詳細ページから Webhook イベントを手動再送できます。Webhook 一覧の "Redeliver" リンクをクリックしてください。
:::

### 返金

返金は [invoice refund endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_Refund) で実行できます。これにより、顧客が返金を受け取るためのリンクが返されます。[Node.Js](https://docs.btcpayserver.org/Development/GreenFieldExample-NodeJS/#issue-a-full-refund-of-an-invoice) と [PHP](https://docs.btcpayserver.org/Development/GreenfieldExample-PHP/#issue-a-full-refund-of-an-invoice) の例はドキュメントを参照してください。

代替として、より汎用的な [pull-payments endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/PullPayments_CreatePullPayment) も利用できます。

## ログ / デバッグ

すべての API コールと、ステータス更新のような内部処理ではエラーを捕捉して適切にログを残すべきです。これにより、BE 側でユーザー/管理者が問題を把握し、実際のエラー情報を添えて問い合わせできます。不要な往復を減らせます。Webhook ペイロードや内部ステータス更新など、より多くのデータを記録する "debug-mode" を用意して、より細かい不具合調査を可能にすることも検討できます。

## データ最小化

請求書のメタデータには任意のデータを渡せますが、十分な理由がない限り顧客データは送るべきではありません。最も重要なメタデータは `orderId` で、これを使って BTCPay の `invoiceId` と関連付けできます。こうすることで顧客データをすべて渡す必要がなくなり、BTCPay Server インスタンスが侵害された場合（またはユーザーが第三者ホストを使っている場合）の情報漏えいリスクを下げられます。
