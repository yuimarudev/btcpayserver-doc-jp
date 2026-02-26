<!-- legacy-anchor-aliases -->
<span id="how-can-i-use-btcpay-in-a-physical-store"></span>
<span id="how-to-create-a-pay-button-with-a-custom-amount"></span>
<span id="how-to-customize-the-appearance-of-point-of-sale-app-in-btcpay"></span>
<span id="how-to-customize-the-appearance-of-Point-of-Sale-App-in-BTCPay"></span>
<span id="how-to-integrate-woocommerce-store-into-a-btcpay-crowdfund-app"></span>
<span id="how-to-map-a-domain-name-to-an-app"></span>
<span id="how-to-redirect-to-another-site-after-payment"></span>
<span id="is-there-a-limit-on-the-number-of-apps-i-can-create"></span>
<span id="is-there-a-point-of-sale-feature-in-btcpay"></span>
<span id="what-are-the-apps-in-btcpay"></span>
<span id="what-is-a-payment-button"></span>
<!-- /legacy-anchor-aliases -->

# Apps FAQ

このドキュメントでは、BTCPay Server の Apps に関するよくある質問を扱います。

[[toc]]

## BTCPay の Apps とは何ですか？

Apps は、BTCPay のユースケースを拡張するために使える機能です。詳細は [apps ドキュメント](../Apps.md) を参照してください。

## 作成できる Apps の数に上限はありますか？

Apps はストア単位で追加されます。作成するには、事前にストアをセットアップしておく必要があります。1つのストアに割り当てられる apps の数に上限はありません。

## BTCPay に Point of Sale 機能はありますか？

はい。 [POS app の作成ガイド](../WhatsNext.md#creating-the-pay-button) をご覧ください。

## 実店舗で BTCPay を使うにはどうすればよいですか？

Point of Sale (PoS) app を利用できます。BTCPay Server 内で PoS app を作成すると、公開 URL でアクセスできるようになり、作成した商品のチェックアウトボタンが表示されます。
実店舗で PoS を使う最も簡単な方法（現時点）は、BTCPay で PoS App を作成し、スマートフォン、タブレット、PC などの Web 端末に表示することです。

詳しくは [モバイル端末での PoS App 利用ガイド](https://blog.btcpayserver.org/bitcoin-pos/) を参照してください。なお、セクション 2.3 の Connecting a Wallet は [ウォレットセクション](../WalletSetup.md) でより詳しく説明されています。

## BTCPay の Point of Sale App の見た目をカスタマイズする方法

Point of Sale app の見た目は簡単にカスタマイズできます。テーマ変更方法は [このガイド](../Development/Theme.md) を参照してください。

## Payment Button とは何ですか？

Payment Button は、作成して Web サイトに埋め込める、シンプルでカスタマイズ可能な HTML ボタンです。作成方法は [このガイド](../WhatsNext.md#creating-the-point-of-sale-app) を参照してください。

## カスタム金額の Pay Button を作るには？

Store Settings > Pay Button にある BTCPay Server Pay Button は、現在カスタム金額をサポートしていません。
ただし、回避策があります。

- [Point of sale app を作成](../WhatsNext.md#creating-the-point-of-sale-app)
- `user can input a custom amount` フィールドを有効化
- 自動生成されたテンプレートから商品をすべて削除
- 設定を保存
- ページ下部の `Embed payment button linking to PoS item` をクリックして展開されたコードをコピーし、Web サイトの html ページに貼り付け
- 不要な追加フィールド、特に `<input name="price" type="hidden" value="10" />` を削除し、ボタンが point of sale にリダイレクトするようにする

![カスタム金額 Pay Button](../img/BTCPayPayButtonDynamic2.png)
![カスタム金額 Pay Button](../img/BTCPayPayButtonDynamic.png)

## ドメイン名を app にマッピングするには？

BTCPay Server Apps には、サーバー本体のドメインとは異なるドメイン名を設定できます。たとえば、BTCPay Server が `mybtcpayserver.com` で動作していて、PoS app を `mybtcpayserver.com/apps/pos/abc123` ではなく `mybtcpaypos.com` に表示したい場合を考えます。
まず、`mypointofsale.com` の [DNS 設定](../FAQ/Deployment.md#setting-up-dns-records)) を行い、BTCPay Server の外部 IP を向くようにしてください。

次に、ssh 経由で新しい環境変数を追加し、追加ドメインまたはサブドメイン名を設定します。

```bash
sudo su -
export BTCPAY_ADDITIONAL_HOSTS="mybtcpaypos.com"
. btcpay-setup.sh -i
```

複数ドメインを追加する場合は、env 変数を再度更新します。

```bash
sudo su -
export BTCPAY_ADDITIONAL_HOSTS="mybtcpaypos.com,subdomain.domain2.com,domain3.com"
. btcpay-setup.sh -i
```

最後に、Server Settings > Policies で `Map specific domains to specific apps` をクリックします。

![App ドメインマッピング](../img/domainmapping1.png)

ドメイン名を入力し、ドロップダウンから事前に作成した app を選択して `save` をクリックすると、そのドメインに app をマッピングできます。

![App ドメインマッピング](../img/domainmapping2.png)

追加したホストのいずれかで DNS が正しく設定されていないと、Let's Encrypt がメインドメインを含むすべてのドメインの証明書を更新できなくなります。追加ホストを使っていてメインドメインで https の問題が出る場合は、`BTCPAY_ADDITIONAL_HOSTS` からドメインを1つ削除し、セットアップを再実行してください。 [Dynamic DNS](/Deployment/DynamicDNS.md) の更新が切れていて追加ホストとして設定されている場合も、https の問題が発生します。

何らかの理由で app を BTCPay Server ホームページと同じドメインに置きたい場合は、ルートに表示する設定を選べます。この場合、ドメインはすでに正しく向いているため DNS 設定は不要です。BTCPay Server のルートドメインで app を使うと、ログインやその他のページに手動でアクセスする必要があります。最も簡単なのは、ルートドメインに `/apps` や `/stores` などのページルートを付ける方法です（例: `mybtcpayserver.com/apps`）。これによりルート表示 app への移動は簡単になりますが、他ページ（ログインなど）への移動は利用者にとって難しくなる可能性があります。

## 支払い後に別サイトへリダイレクトするには？

Point of Sale app では、請求書が支払済みになった後に任意の URL へ顧客をリダイレクトできます。Apps > Settings でリダイレクト機能を変更してください。

![Point of Sale リダイレクト設定](../img/point-of-sale/AppRedirect.png)

PoS 設定における支払済み請求書のリダイレクトオプションは次のとおりです。

- **No** - _Without_ Redirect URL
  - 請求書に、ユーザーを PoS App に戻すための案内が表示されます（デフォルト設定）。
- **No** - _With_ Redirect URL
  - 請求書に、指定した App Redirect URL に戻るための案内が表示されます。
- **Yes** - _Without_ Redirect URL
  - 支払済み請求書が自動的に PoS App にリダイレクトされます。
- **Yes** - _With_ Redirect URL
  - 支払済み請求書が指定した App Redirect URL に自動リダイレクトされます。
- **Use Store Settings**
  - [ストアレベル](../FAQ/Stores.md#how-to-redirect-store-invoices-after-payment) で、PoS App への自動リダイレクトを有効化/無効化します。

補足:

1. Redirect URL は App Settings（リダイレクトオプションの上）で設定してください。
2. 期限切れまたは一部支払い済みの [invoices](../Invoices.md#invoice-statuses) は、設定が有効でもリダイレクトされません。この機能は支払済み請求書専用です。
3. 代わりに API（例: Embedded PoS）経由で redirect URL を指定することもできます。

## WooCommerce ストアを BTCPay の Crowdfund app に統合するには？

支援者にデジタルファイルや物理商品を提供したい場合、WooCommerce ストアを Crowdfunding app に埋め込めます。

![Crowdfunding WooCommerce 統合プレビュー](../img/CrowdfundingWoo.gif)

以下のチュートリアルは、BTCPay、WordPress、WooCommerce について中級程度の理解があることを前提としています。

### 要件

1. Wordpress Website
2. [WooCommerce Plugin](https://wordpress.org/plugins/woocommerce/)
3. [BTCPay for WooCommerce Plugin](https://wordpress.org/plugins/btcpay-for-woocommerce/)
4. [Storefront Theme](https://wordpress.org/themes/storefront/)（別テーマを使っている場合、CSS をテーマに合わせて調整する必要がある場合があります）
5. BTCPay Server

**重要** WooCommerce ストアと BTCPay Server が **同一ドメイン上** にあることを確認してください。ブラウザによってはクロスドメイン埋め込みコンテンツを強くブロックします。特に iOS の Safari では、商品追加時に cookie が破棄され、カートが空になることがあります。これを回避するには、少なくともサブドメインを含めて BTCPay と Woo を同一ドメインに配置する必要があります。

#### 任意の WordPress プラグイン

以下のプラグインは推奨ですが必須ではありません。WordPress の上級ユーザーであれば使用しなくても構いません。

- [Flexible Checkout Fields](https://wordpress.org/plugins/flexible-checkout-fields/)（Woo のチェックアウト編集や不要フィールド削除）
- [WooCommerce Direct Checkout](https://wordpress.org/plugins/woocommerce-direct-checkout/)（チェックアウトの冗長ステップを減らし、支援を素早くする）
- [Header and Footer Scripts](https://wordpress.org/plugins/header-and-footer-scripts/)（`<script>` コードをここに配置）

### 手順

#### 1. 2つのストアを1つのウォレットに接続する

BTCPay Server で、次の2つのストアを別々に作成します。

1. WooCommerce 用ストア
2. Crowdfunding app 用ストア

両方のストアが同期状態を保てるように、**同じ extended public key derivation scheme** を追加します。

#### 2. WordPress の CSS を変更する

最初のステップでは、WordPress ストアから冗長な要素を取り除き、crowdfund app にスムーズに埋め込めるよう、シンプルに整える必要があります。

以下のカスタム CSS を WordPress に追加します。Appearance > Customize > **Custom CSS**

<details>
  <summary>CSS を表示</summary>

CSS file:

```css
#header,
#masthead,
.site-footer,
.storefront-breadcrumb,
.storefront-sorting,
.storefront-product-section .section-title,
.woocommerce-products-header,
.woocommerce-additional-fields,
.woocommerce-form-coupon-toggle,
.woocommerce-breadcrumb,
.related.products {
  display: none;
}

.iframe {
  overflow: hidden;
}

ul.products li.product .button {
  margin-bottom: 0.236em;
  display: block;
}

.product:hover {
  background-color: rgba(0, 0, 255, 0.3);
  color: rgba(0, 0, 0, 0);
  padding-bottom: 45px;
}

.product:hover a * {
  visibility: hidden;
}

.product:hover a.add_to_cart_button {
  position: absolute;
  top: 0;
  left: 0px;
  width: 100%;
  height: 100%;
  padding-top: 50%;
  color: white;
  background-color: rgba(0, 0, 255, 0.3);
}

.product:hover a.add_to_cart_button:hover {
  background-color: rgba(0, 0, 255, 0.5);
}
```

</details>

上記コードは、ストア内の不要要素（ヘッダー、フッター、パンくず、ソート）を削除・非表示にします。Storefront テーマを使っていない場合は、少し調整が必要な場合があります。削除に加えて、コード後半では少し異なるスタイリングを追加し、チェックアウト体験を改善して KickStarter 風にしています。色は自由に変更してください。サイドバーも削除することを推奨します。

WooCommerce チェックアウトの不要フィールド削除には [Flexible Checkout Fields](https://wordpress.org/plugins/flexible-checkout-fields/) を使用してください。

チェックアウトを高速化するには [WooCommerce Direct Checkout](https://wordpress.org/plugins/woocommerce-direct-checkout/) を使います（冗長な手順を削減し、支援をより速く行えるようにします）。

#### 2. WordPress の functions を変更する

子テーマの **functions.php** ファイル末尾に次のコードを追加します。

```php
// Code goes in theme functions.php
add_action( 'after_setup_theme', 'wc_remove_frame_options_header', 11 );

// Allow rendering of checkout and account pages in iframes
function wc_remove_frame_options_header() {
    remove_action( 'template_redirect', 'wc_send_frame_options_header' );
}
```

`Appearance>Editor>functions.php` に直接 php コードを追加すると、次回テーマ更新時に変更が消えます。カスタム function プラグインを使うか、[子テーマを作成](https://docs.woocommerce.com/document/set-up-and-use-a-child-theme/) して、必ずその末尾にコードを配置してください。

#### 3. WordPress にスクリプトを追加する

[Header and Footer Scripts](https://wordpress.org/plugins/header-and-footer-scripts/) プラグインをインストールします。次のコードを header または footer に追加してください。Settings > Headers and Footers Script でコードを貼り付けて保存します。

```html
<script>
  jQuery(document).ready(function () {
    jQuery('.product').each(function () {
      var product = jQuery(this)
      var item = product.find('.woocommerce-loop-product__link')
      var cartLink = product.find('.add_to_cart_button').attr('href')
      item.attr('href', cartLink)
    })
  })
</script>
```

このコードにより、商品エリアのクリックでカート追加が行われ、今回の用途では不要な商品説明ページの表示を防げます。

#### 4. Crowdfunding app を変更する

BTCPay で Apps > Create New App > Crowdfunding を開きます。

app の説明欄でコード表示を有効化し、次のコードを貼り付けて `<iframe src="http://yourdomain/shop/"></iframe>` を追加します。
`yourdomain` は WooCommerce ストアページの URL に置き換えてください。

![EmbedIframeCrowdfund](../img/CrowdfundCodeEmbed.png)

次に、crowdfunding app の **Custom CSS Code** セクションに以下のコードを貼り付けます。

<details>
  <summary>CSS を表示</summary>

CSS file:

```css
#crowdfund-body-header-tagline-container,
#crowdfund-body-description-container {
  max-width: 100% !important;
  width: 100% !important;
  flex: 100%;
}

#crowdfund-body-contribution-container {
  display: none;
}

#crowdfund-body-header-cta {
  display: none;
}

#crowdfund-body-description-container iframe {
  width: 100%;
  border: 0;
  min-height: 500px;
}
/* // Medium devices (tablets, 768px and up) */
@media (min-width: 768px) {
  #crowdfund-body-description-container {
    padding-right: 30%;
    min-height: 1200px;
  }
  #crowdfund-body-description-container iframe {
    width: 30%;
    position: absolute;
    right: 0;
    top: 0;
    height: 100%;
    border-left: 1px #e5e5e5 solid;
  }
}
```

</details>

最後に、**Count all invoices created on the store as part of the crowdfunding goal** に必ずチェック（有効化）してください。
変更を保存し、app をプレビューします。
