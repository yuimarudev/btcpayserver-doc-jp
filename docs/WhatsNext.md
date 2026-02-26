<!-- legacy-anchor-aliases -->
<span id="connecting-your-btcpay-store-to-your-e-commerce-platform"></span>
<span id="creating-the-pay-button"></span>
<span id="creating-the-point-of-sale-app"></span>
<!-- /legacy-anchor-aliases -->

# (4) 次は？

ウォレットを BTCPay に接続したら、ソフトウェア内蔵のさまざまなツールを試せます。Use Cases の全一覧は [here](./UseCase.md) を参照してください。

## Point of Sale アプリの作成

BTCPay には、顧客からの支払いや寄付の受け取りに使える PoS アプリがあります。**POS アプリを作成**するには、BTCPay にストアが作成済みである必要があります。PoS の手順は [here](./Apps.md#point-of-sale-app) を参照してください。

## Crowdfund アプリの作成

**BTCPay を使ったクラウドファンディングキャンペーン**を作成できます。従来のクラウドファンディングプラットフォームと異なり、キャンペーン作成者自身がプラットフォームの所有者です。資金は手数料なしで作成者のウォレットへ直接送られます。Crowdfund の手順は [here](./Apps.md#crowdfunding-app) を参照してください。

## Payment Request の作成

支払いリクエストへのリンクを送ることで、**他者と共有できるカスタム請求書**を作成できます。ユーザーは好きなタイミングで支払えます。BTCPay は支払い時点の BTC 為替レートを自動更新します。Payment Request の手順は [here](./PaymentRequests.md) を参照してください。

## Pay Button の作成

**Pay button** は、商品や寄付の金額が固定の場合に便利です。ボタンは簡単に HTML に埋め込めます。顧客や訪問者がボタンをクリックすると、BTCPay はチェックアウトページと請求書を表示します。Pay Button の手順は [here](./Apps.md#payment-button) を参照してください。

## BTCPay ストアを e コマースプラットフォームへ接続する

利用している CMS に応じて、BTCPay をオンラインストアへ簡単に接続できます。現在、BTCPay は次の連携を提供しています:

- [WooCommerce](./WooCommerce.md)
- [Shopify](./Shopify.md)
- [Drupal](./Drupal/)
- [Magneto](./Magento.md)
- [PrestaShop](./PrestaShop.md)
- [Custom integration](./CustomIntegration.md)
- [Wix](./Wix/)
- [Odoo](./Odoo/)
- [Big Commerce](./BigCommerce/)
- [Invoice Ninja](./InvoiceNinja.md)
e.t.c

## BTCPay Server の拡張: プラグイン

BTCPay Server は単なる決済プロセッサーではありません。好みに合わせてパーソナライズできます。プラグインにより BTCPay Server アプリケーションをカスタマイズし、インスタンスを特定のニーズに合わせられます。

プロジェクトごとに要件は異なります。フリーランス、実店舗やオンラインストアの運営、クリエイティブプロジェクトの管理、開発など、ワークフロー上の課題を解決するプラグインが見つかる可能性があります。

独自機能が必要ですか？BTCPay インスタンスの `Manage Plugin` セクションを開き、自分に合うプラグインを探してください。


## BTCPay コミュニティに参加する

BTCPay Server は企業ではなく、オープンソースプロジェクトです。多数のユースケースに対するサポートは、多様なコントリビューターとユーザーのネットワークに支えられています。BTCPay の改善、学習、構築にぜひ参加してください。

質問がある場合は、まず [FAQ Section](./FAQ/) を検索するか、[BTCPay Community](./Community.md) に参加して質問や改善アイデアを共有してください。

開発者の方は [Local Development](./Development/LocalDevelopment.md) ガイドを確認し、Github の [open issues](https://github.com/btcpayserver/btcpayserver/issues) への対応に協力してください。ほかの形で貢献したい場合は、[Contribution Guide](./Contribute/) を参照してください。
