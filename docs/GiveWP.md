# GiveWP 寄付連携

## 要件

このプラグインをインストールする前に、次の要件を満たしていることを確認してください。

- PHP バージョン 8.0 以上
- cURL、gd、intl、json、mbstring の PHP 拡張が利用可能
- GiveWP がインストールされた WordPress サイト（[インストール手順](https://givewp.com/getting-started/intro-to-givewp/)）
- BTCPay Server バージョン 2.0.0 以降を使用していること（[セルフホスト](/Deployment/README.md) または [サードパーティホスト](/Deployment/ThirdPartyHosting.md)）
- [インスタンスに登録済みアカウントがあること](./RegisterAccount.md)
- [インスタンス上に BTCPay ストアがあること](./CreateStore.md)
- [ストアにウォレットが接続されていること](./WalletSetup.md)

## 1. BTCPay for GiveWP プラグインをインストールする

**BTCPay for GiveWP** プラグインのインストール方法は3つあります。

- WordPress 管理ダッシュボードから（推奨。以下参照）
- [WordPress プラグインディレクトリ](https://wordpress.org/plugins/btcpay-for-givewp/)
- [GitHub Repository](https://github.com/btcpayserver/givewp/releases)

### 1.1 WordPress 管理ダッシュボードからプラグインをインストール（推奨）

1. 左サイドバーで _Plugins_ -> _Add New_ をクリックします。
2. 検索欄で "BTCPay for GiveWP" と入力します。
3. _Install now_ をクリックし、その後 _Activate_ をクリックします。

![BTCPay for GiveWP: Plugin installation](./img/givewp/givewp-install.png)

### 1.2 GitHub からプラグインをダウンロードしてインストール

別の方法として、GitHub からプラグインをダウンロードして手動でインストールできます。

1. [最新の BTCPay プラグインをダウンロード](https://github.com/btcpayserver/givewp/releases) します。
2. WordPress 管理ダッシュボードで _Plugins_ -> _Add Plugin_ をクリックします。
3. _Upload Plugin_ ボタンをクリックし、先ほどダウンロードした .zip ファイルを選択します。
4. _Install Now_ をクリックし、その後 _Activate_ をクリックします。

## 2. GiveWP と BTCPay Server を接続する

BTCPay for GiveWP プラグインは、**BTCPay Server（決済プロセッサ）と寄付フォームをつなぐブリッジ**です。
セルフホストでもサードパーティホストでも、接続手順は同じです。

### 2.1 API キーを作成する

BTCPay Server インスタンス側で（できれば別タブで）以下を実施します。

1. 左下の _[Account]_ -> _Manage Account_ をクリック
2. _"API Keys"_ をクリック
3. _[Generate Key]_ をクリックして権限を選択
4. _"Select specific stores"_ リンクをクリックし、GiveWP ストアを選択して次の権限を設定: `View invoices`, `Create invoice`, `Modify invoices`, `Modify stores webhooks`, `View your stores`, `Create non-approved pull payments`（返金用途。未実装）
   ![BTCPay for GiveWP: API Keys Permissions](./img/givewp/btcpay-api-key-1of2.png)
   ![BTCPay for GiveWP: API Keys Permissions](./img/givewp/btcpay-api-key-2of2.png)
5. 右上の _[Generate API Key]_ をクリック
6. 生成された API Key と Store ID を安全な場所に保存します。次の手順で使用します。
   ![BTCPay for GiveWP: API Keys Save](./img/givewp/btcpay-api-key-success.png)

### 2.2 Store ID をコピーする

引き続き BTCPay Server 側で:

1. 左サイドバーのストアドロップダウンで、GiveWP に接続したいストアを選択
2. 同じく左サイドバーの _[Settings]_ をクリック
3. ページ上部に _Store ID_ が表示されます。
   ![BTCPay for GiveWP: Copy Store ID](./img/givewp/btcpay-store-id.png)
4. Store ID を安全な場所に保存します。次の手順で使用します。

### 2.3 GiveWP 設定に API キーと Store ID を入力する

WordPress サイト側に戻って:

1. WordPress ダッシュボードを開きます。
2. サイドバーの _GiveWP_ -> _Settings_ -> _Payment Gateways_ へ移動します。
3. _BTCPay Gateway_ タブをクリックします。
4. _BTCPay Server URL_ に BTCPay Server の URL（例: `https://btcpay.example.com`）を入力します。
5. GiveWP の _BTCPay Settings_ に Store ID を入力します。
6. GiveWP の _BTCPay Settings_ に生成した API Key を入力します。
   ![BTCPay for GiveWP: Copy API Key](./img/givewp/givewp-settings.png)
7. ページ下部の _[Save changes]_ をクリックします。
8. ページ上部に "_BTCPay for GiveWP: BTCPay Server API credentials verified successfully._" と "BTCPay for GiveWP: Webhook created successfully." の通知が表示されることを確認します。
   ![BTCPay for GiveWP: Save BTCPay Settings form saved](./img/givewp/givewp-settings-success.png)
9. ページ上部の [Gateways] リンク/タブをクリックしてゲートウェイ一覧に戻ります。

10. ゲートウェイ一覧に _BTCPay Server Gateway_ が利用可能な支払い方法として表示されているはずです。
11. "Enabled" 列にチェックを入れて BTCPay Server Gateway を有効化します。"Default" 列にチェックを入れると既定ゲートウェイにもできます。
    ![BTCPay for GiveWP: Gateways Overview](./img/givewp/givewp-settings-gateway-default.png)

これで GiveWP 寄付フォームで BTCPay Server による寄付受付の準備が完了です。

## 3. 寄付決済をテストする

ストアで少額のテスト寄付を行うと安心です。
本番公開前には、設定が正しいことを必ず確認してください。

![BTCPay for GiveWP: Test Donation](./img/givewp/givewp-bitcoin-payment-option.png)
![BTCPay for GiveWP: Test Donation payment page](./img/givewp/givewp-payment-page.png)
![BTCPay for GiveWP: Test Donation](./img/givewp/givewp-donation-paid.png)

## サポート
[リポジトリ](https://github.com/btcpayserver/givewp) で issue を作成するか、[Telegram](https://t.me/btcpayserver) または [Mattermost chat](http://chat.btcpayserver.org/) でご連絡ください。
