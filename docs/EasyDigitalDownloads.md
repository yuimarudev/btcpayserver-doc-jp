# Easy Digital Downloads (EDD) 統合

:::warning
この統合は BTCPay Server チームによって保守されていない点に注意してください。機能要望やバグ報告は、[coinsnap repository](https://github.com/Coinsnap/Coinsnap-for-EasyDigitalDownloads) に直接投稿してください。
:::
[![Easy Digital Downloads Bitcoin](https://img.youtube.com/vi/qAenGKJYM8I/mqdefault.jpg)](https://www.youtube.com/watch?v= qAenGKJYM8I)
## 要件

このプラグインをインストールする前に、次の要件を満たしていることを確認してください。

- PHP version 8.0 or newer
- The cURL, gd, intl, json, and mbstring PHP extensions are available
- Easy Digital Downloads (EDD) がインストールされた WordPress サイト（[installation instructions](https://easydigitaldownloads.com/docs/quickstart-guide/)）
  注: 開始に EDD の Pro 版は不要です
- [self-hosted](/Deployment/README.md) または [hosted by a third-party](/Deployment/ThirdPartyHosting.md) の BTCPay Server バージョン 2.0.0 以降を利用していること
- [インスタンスに登録済みアカウントがあること](./RegisterAccount.md)
- [インスタンス上に BTCPay ストアがあること](./CreateStore.md)
- [ストアにウォレットが接続されていること](./WalletSetup.md)

## 1. Bitcoin for Easy Digital Downloads プラグインをインストール

[Coinsnap](https://coinsnap.io) の **Bitcoin for Easy Digital Downloads** プラグインにより、BTCPay Server へ接続できます。

プラグインのインストール方法は3つあります。

- WordPress の Admin Dashboard からインストール（推奨、以下参照）
- [WordPress plugin directory](https://wordpress.org/plugins/coinsnap-for-easy-digital-downloads/)
- [GitHub Repository](https://github.com/Coinsnap/Coinsnap-for-EasyDigitalDownloads)

### 1.1 WordPress Admin Dashboard からインストール（推奨）

1. 左サイドバーで _Plugins_ -> _Add New_ をクリックします。
2. Search に「easy digital downloads btcpay」と入力します。
3. _Install now_ をクリックし、その後 _Activate_ をクリックします。

![Bitcoin for EDD: Plugin installation](./img/edd/edd-search-and-install-plugin.png)

### 1.2 GitHub からダウンロードしてインストール

別の方法として、GitHub からプラグインをダウンロードして手動でインストールできます。

1. [プラグインリポジトリを開きます](https://github.com/Coinsnap/Coinsnap-for-EasyDigitalDownloads)。
2. _Code_ -> _Download ZIP_ をクリックして .zip をダウンロードします。
3. WordPress admin dashboard で _Plugins_ -> _Add Plugin_ をクリックします。
4. _Upload Plugin_ ボタンをクリックし、先ほどダウンロードした .zip ファイルを選択します。
5. _Install Now_ をクリックし、その後 _Activate_ をクリックします。

## 2. EDD と BTCPay Server を接続

Bitcoin for EDD プラグインは、**BTCPay Server（決済プロセッサ）と EDD ストアをつなぐブリッジ**です。
self-hosted でも第三者ホスティングでも、接続手順は同じです。

### 2.1 EDD で Bitcoin サポートを有効化

:::info
上記インストール後、EDD の payment gateways に「Coinsnap」として表示されます。
:::

1. WordPress 管理 UI で、左サイドバーの EDD（Downloads）セクション内にある _[Settings]_ をクリックします。
2. 上部の _"Payments"_ タブをクリックします。
3. _Coinsnap_ のトグルを有効化します。
4. 下部の _[Save Changes]_ ボタンをクリックします。

![Bitcoin for EDD: Enable payment gateway](./img/edd/edd-setup-enable-gateway.png)

### 2.2 Coinsnap ゲートウェイを設定

1. Coinsnap 設定フォームを開いていることを確認します。違う場合は上部の _"Coinsnap"_ タブをクリックします。
2. "Payment provider" フィールドで _"BTCPay Server"_ を選択します。
3. _"BTCPay Server URL"_ 入力欄に、BTCPay Server インスタンスの URL（例: `https://btcpay.example.com`）を入力します。
4. _[Generate API key]_ ボタンをクリックします。

BTCPay Server インスタンスへリダイレクトされるので、以下の手順に従います。

![Bitcoin for EDD: Start setup wizard](./img/edd/edd-setup-configure.png)

### 2.3 BTCPay Server 側: プラグインアクセスを認可

BTCPay Server インスタンスで次を実施します。

1. 認可ページでストアを選択します。ここでは「EDD」を選び、_[Continue]_ をクリックします。
   ![Bitcoin for EDD: Authorize select store](./img/edd/edd-setup-authorize-select-store.png)
2. 次の画面でプラグインに必要な権限が表示されます。ラベルを入力し、下部の _[Authorize app]_ ボタンをクリックします。
   ![Bitcoin for EDD: Authorize plugin access](./img/edd/edd-setup-authorize-permissions.png)
3. EDD 設定フォームへ戻ります。"Connection status" が BTCPay Server 接続済みとなり、"Store ID" と "API key" フィールドが自動入力されているはずです。
   ![Bitcoin for EDD: Configure completed](./img/edd/edd-setup-completed.png)
4. 保存を確実にするため、下部の _[Save Changes]_ ボタンをクリックします。

これで、BTCPay Server 経由でダウンロード商品を Bitcoin 決済できるようになります。

## 3. チェックアウトのテスト

ストアで少額のテスト決済を行うと安心です。
本番公開前に、すべてが正しく設定されていることを必ず確認してください。

Checkout で注文を作成します。
![Bitcoin for EDD: Test purchase](./img/edd/edd-checkout.png)

BTCPay Server にリダイレクトされ、請求書の qr-code が表示されます。
![Bitcoin for EDD: Test purchase invoice](./img/edd/edd-checkout-invoice.png)

請求書を支払った後、サイトへ戻れます。
![Bitcoin for EDD: Test purchase invoice paid](./img/edd/edd-checkout-invoice-paid.png)

支払い完了ステータス付きの注文確認ページが表示されます。
![Bitcoin for EDD: Test purchase redirect order confirmation](./img/edd/edd-checkout-invoice-paid.png)

管理画面の "_Downloads_" -> _"Orders"_ でも、注文が完了していることを確認できます。
![Bitcoin for EDD: Test purchase redirect order confirmation](./img/edd/edd-admin-order-completed.png)

## サポート
[Coinsnap Github repository](https://github.com/Coinsnap/Coinsnap-for-EasyDigitalDownloads) で issue を作成するか、[Telegram](https://t.me/btcpayserver) または [Mattermost chat](http://chat.btcpayserver.org/) で連絡できます。
