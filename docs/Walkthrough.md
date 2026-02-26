# BTCPay Server ウォークスルー

このページでは、**BTCPay のユーザーインターフェース**をたどりながら、さまざまなオプションの操作方法を説明します。

機能のインタラクティブな概要は、以下の動画をご覧ください。

[![BTCPay Server Walkthrough](https://img.youtube.com/vi/ZIfJyq9RimM/mqdefault.jpg)](https://www.youtube.com/watch?v=ZIfJyq9RimM)

自分でホストした、または第三者がホストした BTCPay Server インスタンスでアカウントを作成すると、ストアの新しいホーム画面、つまり `dashboard` が表示されます。

![BTCPay Server Navigation](./img/walktrough/welcome-store.jpg)

左側メニューの設定はすべて、上部で選択した現在のストアに対して適用されます。

![BTCPay Server Navigation](./img/walktrough/selected-store.jpg)

- [通知](Walkthrough.md#notifications)
- [ダッシュボード](Dashboard.md)
- [設定](Walkthrough.md#store) **ストア設定**
- [ウォレット](Walkthrough.md#wallets)
  - Bitcoin
  - Lightning
- [支払い](Walkthrough.md#payments)
  - 請求書
  - 支払いリクエスト
  - Pull Payments
  - Payouts
  - Pay Button
- [アプリ](Walkthrough.md#apps)
  - 新しいアプリ
- [プラグイン](Walkthrough.md#plugins)
  - プラグイン管理
- [サーバー設定](Walkthrough.md#server-settings)
- [アカウント](Walkthrough.md#account)

<a id="store"></a>
## ストア

BTCPay では、**無制限の数のストアを作成・管理**できます。各ストアは独自のウォレットを持ち、アプリ（Point of Sale、Payment Button、Crowdfund）を作成したり、利用可能な統合機能を通じて外部の e コマースソフトウェアと連携できます。管理者は他ユーザーのストアの秘密鍵を管理できません。詳細は [Stores FAQ](./FAQ/Stores.md) を参照してください。

- Store settings - グローバルな支払い設定を構成し、顧客向けの支払い体験をカスタマイズします。
- Rates - ストアの暗号資産と法定通貨の[為替レート](./FAQ/Stores.md#how-to-change-the-exchange-rate-provider-for-invoices)の取得元を設定します。
- Checkout experience - チェックアウトページの[見た目をカスタマイズ](./FAQ/ServerSettings.md#how-to-modify-the-checkout-page)し、既定コインなどを選択します。
- Access Tokens - [統合機能とストアをペアリングする](./WhatsNext.md#connecting-your-btcpay-store-to-your-e-commerce-platform)ためのトークンです。
- Users - BTCPay アカウントを持つ他ユーザーに、ゲストまたはオーナーとしてストアへのアクセスを許可します。
- Pay Button - Web サイトに簡単に埋め込める[支払いボタンを作成](./WhatsNext.md#creating-the-pay-button)します。

<a id="notifications"></a>
## 通知

通知は、**BTCPay Server インスタンスでイベントが発生した**ことをユーザーに知らせます。
イベントには、支払いの受領や失敗、請求書の過払い/不足払い、新しい BTCPay バージョンの公開などがあります。

アイコンをクリックすると `Notifications` ページに移動し、過去の通知の確認や必要に応じた削除ができます。
BTCPay の通知については [こちら](./Notifications.md) を参照してください。

## ダッシュボード

ダッシュボードでは、ストアのウォレット残高、請求書の概要、進行中のクラウドファンディングの上位特典をすばやく確認できます。
ダッシュボードには主要な 5 つのタイルがあります。

- ウォレット残高のクイック表示
- トランザクションと出金アクティビティ
- 最近のトランザクション
- 最近の請求書
- 現在進行中のクラウドファンディング

詳細は [Dashboard](Dashboard.md) を参照してください。

<a id="wallets"></a>
## ウォレット

### Bitcoin

設定している支払い方法の数に応じて、ウォレットタブには各支払い方法ごとのウォレットが表示されます。Bitcoin オンチェーンウォレットでは、受け取った資金を管理できます。BTCPay のウォレットは機能が豊富で、プライバシー機能も組み込まれています。さらにハードウェアウォレット統合にも対応しており、互換性のあるハードウェアウォレットで BTCPay から直接資金管理が可能です。詳細は [wallet page](Wallet.md) を参照してください。

内部 BTCPay ウォレットの要素は次のとおりです。

- Transaction - すべてのトランザクション履歴を表示します。
- Send - ウォレットから資金を送金するために使用します（互換性のあるハードウェアウォレットで署名と承認が必要です）。
- Receive - 新しいアドレスを手動で生成するために使用します。
- Rescan - 古いウォレットをより簡単に BTCPay に取り込み、外部ウォレットによくあるギャップリミット問題を解決します。
- Pull Payments - Pull Payments の作成と管理に使用します。詳細は [Pull Payments](PullPayments.md) を参照してください。
- Payouts - Pull Payment リクエストの管理に使用します。
- PSBT - PSBT 標準を通じてマルチシグトランザクションに署名するために使用します。
- Settings - ウォレットの追加設定を表示・調整するために使用します。

### Lightning

加えて、Lightning ウォレットの追加を推奨します。選択肢は 2 つあり、[内部](./LightningNetwork.md#connecting-your-internal-lightning-node-in-btcpay)を接続するか、外部の [Lightning node](./LightningNetwork.md) を接続できます。
完了すると、Lightning ウォレット機能が有効になります。

詳細は [Wallet](./Wallet.md) または [Wallet FAQ](./FAQ/Wallet.md) を参照してください。

<a id="payments"></a>
## 支払い

### 請求書

あなたのユーザーアカウントに紐づく**すべての請求書**がここに表示されます。ステータス、注文、商品、ストア、日付で絞り込みできます。請求書の手動作成も可能です。請求書は新しい順に並びます。個別の請求書を開くと詳細を確認できます。エクスポートボタンでファイル（`.json` または `.csv`）を保存できます。

詳細は [Invoices](Invoices.md) を参照してください。

### 支払いリクエスト

各ストアには、ここに表示される**支払いリクエスト**を無制限に作成できます。支払いリクエストは、URL で共有でき、現在の BTC 為替レートでいつでも支払える動的な請求書です。ここでは支払いリクエストの編集と表示ができ、請求書詳細の確認や過去の支払いリクエストの複製も行えます。

詳細は [Payment Requests](PaymentRequests.md) を参照してください。

### Pull Payments

[pull payments](PullPayments.md) 機能は、
サブスクリプションサービス、返金、フリーランサー向けの時間課金、パトロネージ、出金サービスなどに適しています。

概念の詳細な説明は [Pull Payments](PullPayments.md) を参照してください。

### Payouts

`payouts` ビューでは、現在の pull payments とそのステータスを一覧で確認できます。
たとえば返金が発行され、請求者が承諾した場合、この情報が Payouts に表示されます。
ここでは返金要求額の承認と直接送金が可能です。
複数の Pull payments がある場合は、選択してまとめて一括送信できます。
将来のバージョンでは、スケジュール機能の追加を予定しています。

![BTCPay Server Navigation](./img/walktrough/walktrough-payouts1.jpg)

### Pay Button

Web サイトの HTML に、寄付ボタンや支払いボタンを簡単に埋め込めます。
顧客または訪問者がボタンをクリックすると、BTCPay がチェックアウトページと請求書を表示します。

詳細は [Create a payment button](./Apps.md#payment-button) を参照してください。

![BTCPay Server Navigation](./img/walktrough/preview-paybutton.jpg)

<a id="apps"></a>
## アプリ

各ストアは複数のアプリを利用できます。**BTCPay 上に構築されたアプリケーション**は、ソフトウェアの[ユースケース](./UseCase.md)を拡張し、さまざまな種類のユーザーに対応します。ここでは新しいアプリを作成し、ストアに接続し、カスタマイズできます。代表例は Point of Sale アプリで、実店舗での支払いや寄付の受け取りに利用できます。

詳細は [Apps](./Apps.md) または [Apps FAQ](./FAQ/Apps.md) を参照してください。

<a id="plugins"></a>
## プラグイン

この画面から、ストアで使用するプラグインを管理できます。
ユーザーが利用可能なプラグインはサイドメニューに表示されます。

詳細は [Integrations](./CustomIntegration.md)
または以下のビルド済みプラグインを参照してください。

- [WooCommerce](WooCommerce.md)
- [Shopify](Shopify.md)
- [Magento](Magento.md)
- [Prestashop](PrestaShop.md)
- [Drupal](/Drupal/)
- [Zapier](./Zapier/README.md)

<a id="server-settings"></a>
## サーバー設定

**Server settings** はサーバー管理者のみがアクセスできます。他者のサーバーを利用している場合は Server Settings は表示されません。設定内では、ユーザー管理、レート設定、サーバー更新などを実行できます。詳細は [Server Settings FAQ](./FAQ/ServerSettings.md) を参照してください。

- Users - BTCPay Server のユーザーを追加、削除、管理します。
- Email server - 登録時にメールアドレス確認を求める場合は SMTP 設定を行います。
- Policies - ユーザー登録、メール認証、検索エンジンのインデックス、サイトルートでのアプリ表示を有効/無効化します。
- Services - gRPC、REST、RTL は LN ノード接続、SSH キー、アップロードファイル保存設定で使用します。
- Theme - BTCPay Server のフロントエンド外観をカスタマイズします。
- Maintenance - BTCPay を最新バージョンに更新し、未使用の docker イメージを削除してクリーンアップします。
- Logs - BTCPay Server の最新ログを表示します。
- Files - Services でこの機能を有効化後、外部ファイルをアップロードして URL でアクセスできます。

<a id="account"></a>
## アカウント

BTCPay Server アカウントを管理します。
ユーザーアカウントに関する項目を変更できます。
二要素認証の設定や API キー管理も行えます。

## BTCPay コミュニティに参加

疑問がある場合は、[FAQ セクション](./FAQ/)を検索するか、[BTCPay Community](./Community.md) に参加して質問や改善アイデアを共有してください。

開発者の方は [Local Development](./Development/LocalDevelopment.md) ガイドを確認し、GitHub の [open issues](https://github.com/btcpayserver/btcpayserver/issues) への対応にご協力ください。ほかの形で BTCPay に貢献したい場合は、[Contribution Guide](./Contribute/) のアイデアを参照してください。
