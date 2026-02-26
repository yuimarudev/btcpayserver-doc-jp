<!-- legacy-anchor-aliases -->
<span id="how-to-configure-additional-token-support"></span>
<span id="integrations-general-faq"></span>
<span id="woocommerce-faq-2"></span>
<!-- /legacy-anchor-aliases -->

# Integrations FAQ

このページでは、BTCPay の連携に関する質問を扱います。

[[toc]]

## General Integrations FAQ

### What e-commerce integrations are available?

- [WooCommerce](../WooCommerce.md)
- [Drupal](../Drupal/)
- [Magento](../Magento.md)
- [PrestaShop](../PrestaShop.md)
- [カスタム連携](../CustomIntegration.md)

開発者であれば、[カスタム連携の手順](../CustomIntegration.md)に従って独自の連携を開発できます。

### How to use WooCommerce store with BTCPay?

- [BTCPay と WooCommerce](https://www.youtube.com/watch?v=tTH3nLoyTcw)
- [BTCPay WordPress プラグインのインストール](https://www.youtube.com/watch?v=6QcTWHRKZag)
- [ストアをサードパーティ BTCPay ホストに接続する](https://www.youtube.com/watch?v=IT2K8It3S3o)
- [ウォレットを BTCPay に接続する](https://www.youtube.com/watch?v=xX6LyQej0NQ)
- [設定完了後にストアのチェックアウトをテストする](https://www.youtube.com/watch?v=Fi3pYpzGmmo)

### How to use BTCPay with Drupal?

- [BTCPay と Drupal のインストールと設定](https://github.com/btcpayserver/commerce_btcpay#installation-and-configuration)
- [Drupal Commerce BTCPay モジュールのインストール手順](https://youtube.com/watch?v=XBZwyC2v48s)

### How to use BTCPay with Prestashop?

- [PrestaShop 用 BTCPay プラグインの利用](../PrestaShop.md)

### Does BTCPay have a Shopify plugin?

はい、BTCPay と Shopify の連携があります。開始するには、[Shopify 連携ガイド](../Shopify.md)を確認してください。

### Can I use BTCPay without an integration?

はい、可能です。さまざまな e コマース CMS では連携を利用しますが、加盟店でなくても BTCPay を利用できます。ユースケースの詳細は[こちらのページ](../UseCase.md)を参照してください。

## WooCommerce FAQ

### How to upgrade to the new BTCPay for WooCommerce V2 plugin?

[旧 BitPay ベースのレガシープラグイン](https://wordpress.org/plugins/btcpay-for-woocommerce/)から新しい [V2 バージョン](https://wordpress.org/plugins/btcpay-greenfield-for-woocommerce/)への直接アップグレードはありません。代わりに完全に別のプラグインをインストールします（インストール手順は[こちら](../WooCommerce.md)を参照）。両方を並行して動作させることはできますが、インストール手順に従って正常動作を確認した後は、レガシープラグインをアンインストールすることを強く推奨します。そうしないと、構成によっては意図しない挙動や混乱を招く可能性があります。

### How to configure order status in WooCommerce?

注文ステータスは、加盟店のビジネスモデルに依存します。BTCPay の注文（請求書）ステータスを理解するには、[このドキュメント](../WooCommerce.md#btcpay-order-statuses)を参照してください。
試行錯誤して自分のビジネスに合う設定を見つける以外に、最適な方法はありません。

### How to customize e-mail confirmations in WooCommerce?

特定のステータスの後に顧客へメールを送信したい場合は、WooCommerce の注文メールテンプレートを編集する必要があります。これは内容を理解している場合にのみ推奨されます。[このガイド](https://www.cloudways.com/blog/how-to-customize-woocommerce-order-emails/)を参照してください。

### Error: If you use an alternative order numbering system, please see class-wc-gateway-btcpay.php to apply a search filter

:::warning Warning
このガイドは現在廃止された[レガシープラグイン](https://wordpress.org/plugins/btcpay-for-woocommerce/)向けであり、最新の[プラグイン V2](https://wordpress.org/plugins/btcpay-greenfield-for-woocommerce/)には適用されません。
:::

もし WooCommerce で標準とは異なる注文番号方式を使っている場合、BTCPay WooCommerce プラグインのログに次のエラーが表示されることがあります。

> [Error] The BTCPay payment plugin was called to process an IPN message but could not retrieve the order details for order_id: "ON123". If you use an alternative order numbering system, please see class-wc-gateway-btcpay.php to apply a search filter.

以下のコードを子テーマの **functions.php** ファイル末尾に貼り付けてください。

<details>
  <summary>コードスニペットを表示</summary>

```php
function get_order_id_from_custom_order_style($orderid){
  if(is_string($orderid)){
    $result = preg_replace('~\D~', '', $orderid);
    return $result;
  }
  return $orderid;
}

add_filter('woocommerce_order_id_from_number', 'get_order_id_from_custom_order_style', 1);
```

</details>

### How to configure Additional Token Support / Separate Payment Gateways

:::warning Warning
このガイドは現在廃止された[レガシープラグイン](https://wordpress.org/plugins/btcpay-for-woocommerce/)向けであり、最新の[プラグイン V2](https://wordpress.org/plugins/btcpay-greenfield-for-woocommerce/)には適用されません。V2 プラグインではこの機能は現在「**Separate Payment Gateways**」と呼ばれます。以下のユースケースは引き続き有効ですが、[Setup your additional tokens](#setup-your-additional-tokens) の手順に従う必要は**ありません**。代わりに、機能を有効化してサポートされるトークンを BTCPay Server インスタンスから自動取得する[設定項目](../WooCommerce.md#41-global-settings)が利用できます。
:::

::: tip Note
この連携で使用する外部の WordPress / WooCommerce プラグインは、BTCPay Server チームによる推奨や、十分に検証・監査されたものではありません。利用は自己責任で行ってください。
:::

追加トークン設定を使うと、構成した各通貨・アセット・アルトコイン・トークンごとに個別の支払い方法を持てます。つまり、BTC、Lightning Network、LTC、ETH（および ERC20 トークン）、Liquid アセットなどをそれぞれ別の支払い方法として提供できます。これにより、Liquid アセットをクーポンやバウチャーとして発行して利用することも可能です。詳細は以下を参照してください。

#### Use cases

- プロモーショントークンで商品を無料配布する
- 特定の支払い方法（トークン）に割引を適用する
- 特定の支払い方法（トークン）に商品の購入を制限する
- 配送ゾーンごとに利用可能な支払い方法（トークン）を制限する
- その他多数（以下の例を参照）

#### Requirements

- WooCommerce 側で設定するすべてのトークンは、BTCPay Server 側のストアでも利用可能である必要があります
- プロモーショントークンを使うには、BTCPay Server に [Liquid Assets plugin](https://github.com/btcpayserver/btcpayserver-plugins) をインストールしておく必要があります

#### Token types

##### Payment tokens

支払いトークンは、BTCPay Server が標準でサポートしているもの（BTC、Lightning Network、LTC、XMR など）です。これらはショップの法定通貨に対する現在の為替レートで換算され、通常の支払い通貨として使われます。

##### Promotional tokens (100% discount)

前述の Liquid Assets plugin の導入により、**プロモーショントークン**も受け付けられるようになります。これは商品やギフトの引き換えに使えるクーポンやバウチャーのようなものです。特徴として小数を持たず、商品数量 1 つあたり常に 1 トークンで支払う必要があります。

ストアオーナーとして、この用途向けに[独自の Liquid アセットを発行](https://docs.blockstream.com/liquid/developer-guide/developer-guide-index.html#issued-assets)することも、[既存のもの](https://blockstream.info/liquid/assets)を受け入れることもできます。

#### Configuration

WooCommerce ストアで設定するトークンが BTCPay Server 側で利用可能かつ正しく設定されていることを確認してください。そうでない場合、チェックアウト時の請求書作成でエラーが発生します。将来的には Greenfield API 経由で必要データを直接取得する新しい WooCommerce プラグインで改善される予定ですが、現時点ではカンマ区切り値（CSV）形式でデータを入力する必要があります。

##### Preparation

最新の WooCommerce プラグインがインストールされていることを確認してください。

##### Setup your additional tokens

###### Setting: Additional token configuration

BTCPay の支払い方法設定には、4 列の特定 CSV 形式でトークン設定を入力できる新しい設定 **「Additional token configuration」** があります。

1. **token symbol**:
   重要: これは BTCPay Server 上のシンボル（例: BTC）と一致している必要があります。

2. **display name**:
   チェックアウト時に表示される支払い方法のテキストです。

3. **type**:
   これは「**payment**」または「**promotion**」を指定します（[上の説明](#token-types)を参照）。

4. **token icon (optional)**:
   チェックアウト時に表示されるトークンシンボルの URL（空でも可。ただし引用符は含めてください）。メディアマネージャーにアイコンをアップロードして URL をコピーするか、外部サイトや CDN へのリンクを使えます。

:::danger
**重要:** すべての列のテキストは二重引用符 `"` で囲み、セミコロン `;` で区切る必要があります。各アセットは改行して 1 行ごとに記述してください。
:::

**追加トークン設定の例**

```
"BTC_OFFCHAIN";"Lightning BTC";"payment";""
"USDt";"USDt (Liquid Theter)";"payment";"https://example.com/wp-content/uploads/2021/01/usdt.png"
"eKr";"eKrona (Liquid Asset)";"promotion";""
```

保存後、各アセットが支払い方法として利用可能になります。他の支払い方法と同様に有効/無効を切り替えられます。現時点では個別設定はありません（すべて CSV データで設定されます）。ただし、WooCommerce の支払い系プラグインなどと組み合わせて、特定の支払い方法に割引を適用するなどの運用ができます。

![追加した各トークンが支払い方法として利用可能](../img/woocommerce/woocommerce_at_payment-methods.png)

###### Setting: Additional tokens: Enforce payment tokens

BTCPay Server のデフォルト支払い方法（Bitcoin）は、設定済みの通貨・アセット・アルトコイン・トークンを**制限しません**。つまり、デフォルトの支払い方法「Bitcoin」を有効化している場合、ユーザーは BTCPay Server の支払いページで、設定済みの通貨・アセット・アルトコイン・トークン（為替レートがあるもの）をすべて選択できます。これを望まない場合は、利用可能な支払いオプションを制限できます。このチェックボックスを選択すると、[Setting: Additional token configuration](#setting-additional-token-configuration) に列挙した type が "payment" の通貨・アセット・アルトコイン・トークンのみに制限されます。

#### Common WooCommerce use-cases using the Additional Token Support feature

##### Use-case 1: limit product to a region/shipping zone

使用する無料プラグイン: [Country Based Restrictions for WooCommerce](https://wordpress.org/plugins/woo-product-country-base-restrictions/)
プラグインをインストールして有効化した後、商品の「Product data」ブロックに新しい「Country restrictions」タブが表示されます。そこで希望する制限を設定できます。

設定例:
![商品を米国のみに制限](../img/woocommerce/woocommerce_at_product-country-restriction.png)

##### Use-case 2: (Promotion) products should have free shipping

これにより、顧客が特定の通貨・アセット・アルトコイン・トークンで支払った場合に送料無料を提供できます。
これは WooCommerce の標準機能だけで実現可能です（プラグイン不要）。

1. 配送設定で新しい配送クラス（例: 「free-shipping」）を追加します
2. 配送ゾーン / 配送方法の設定で、その配送クラスの料金を 0 にし、かつ「cost」を空または 0 に設定してください。また「no shipping class cost」は通常料金に設定します（以下は flat-rate の例）。
   ![フラットレートでの送料無料設定例](../img/woocommerce/woocommerce_at_free-shipping-flat-rate-config.png)
3. 商品設定の「Product data」ブロックにある「Shipping」タブで、上で作成した「Free-shipping」クラスを設定すると、チェックアウト時に適用されます。
   ![商品設定で送料無料クラスを設定](../img/woocommerce/woocommerce_at_free-shipping-product-setting.png)

##### Use-case 3: limit product payment methods

例: プロモーション商品に対して、特定の通貨・アセット・アルトコイン・トークンだけを支払いとして許可する

使用する無料プラグイン: [Conditional Payments for WooCommerce](https://wordpress.org/plugins/conditional-payments-for-woocommerce/)

このプラグインは条件ルールビルダーを提供し、商品ごとに利用可能な支払い方法を有効/無効にできます。設定例はスクリーンショットを参照してください。
![条件付き支払いルールの概要](../img/woocommerce/woocommerce_at_limit-payment-methods-rules.png)

##### Use-case 4: discount per payment method

顧客が特定の通貨・アセット・アルトコイン・トークンで支払う場合に、割引を提供できるようになります。

使用する無料プラグイン: [Discounts Per Payment Method for WooCommerce](https://wordpress.org/plugins/woo-payment-discounts/)

WooCommerce 設定内の「Discount per Payment」設定で、すべての支払い方法に対して割合または固定額の割引を指定できます。

![利用可能な支払い方法ごとの割引設定](../img/woocommerce/woocommerce_at_payment-method-discount.png)

##### Use-case 5: make sure promotional products can only be purchased exclusively

これは、特定の通貨・アセット・アルトコイン・トークンに基づく支払い方法（プロモーショントークンとして利用）で、商品価格を 1（数量ごと）に上書きし、ユーザーが数量 1 につきプロモーショントークン 1 で支払えるようにするために必要です。これを行わないと、通常商品とプロモ商品を同一チェックアウトで混在させ、両方をプロモーショントークンで支払えてしまう可能性があり、これは避けたい挙動です。

商品設定の右サイドバーにある「Product tags」で、新しいタグ「promotion」を追加します。

![商品編集画面で promotion タグを設定](../img/woocommerce/woocommerce_at_product_promotion_tag.png)

以下のコードを子テーマの **functions.php** ファイル末尾に貼り付けてください。

<details>
  <summary>コードスニペットを表示</summary>

```php
/**
* Check if a product is tagged with "promotion" and show a notice that it only
* can be ordered exclusively without any other products in the cart.
*/
function btcpay_check_promotion_product($valid, $product_id, $quantity) {
  $promotion_tag = 'promotion';
  // Check if there are any items in the cart.
  if (!empty($cart_items = WC()->cart->get_cart()) && $valid) {
    // Check if the product is a promotional product and abort.
    if (has_term($promotion_tag, 'product_tag', $product_id)) {
      wc_add_notice( 'Promotional products can only be purchased exclusively, please remove other items from your cart first.', 'error' );
      return false;
    }
    // Also check the case where one has already a promotion product in the
    // cart and also do not allow adding a normal product in that case.
    foreach ($cart_items as $item) {
      if (has_term($promotion_tag, 'product_tag', $item['product_id'])) {
        wc_add_notice( 'Promotional products can only be purchased exclusively, please proceed with checkout or remove the item first.', 'error' );
        return false;
      }
    }
  }

  return $valid;
}
add_filter('woocommerce_add_to_cart_validation', 'btcpay_check_promotion_product', 10, 3);
```

</details>

##### Use-case 6: Limit the checkout of only 1 piece of a product

これにより、1 回のチェックアウトで顧客が使用できる通貨・アセット・アルトコイン・トークンの数量を制限できます。

1 回のチェックアウトにつき 1 回だけ割引を適用したいクーポン型プロモーションに有用です。

これは WooCommerce ですでに対応可能です。商品ごとに Product settings の「**Inventory**」タブで有効化できます。
チェックボックス [x] 「_Enable this to only allow one of this item to be bought in a single order_」を設定してください。
