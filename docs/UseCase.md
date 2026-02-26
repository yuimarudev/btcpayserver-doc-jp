---
description: BTCPay Server は誰が、なぜ使うのか？他の決済ゲートウェイと比べた利点は何か？
tags:
  - Use-case
  - Target-audience
  - BTCPay users
  - benefits
---

# BTCPay Server は誰が使えるのか？

BTCPay Server の汎用性と柔軟性は、さまざまなタイプのユーザーを引きつけます。BTCPay Server は **誰でも** 利用できます。

私たちは、地理的・政治的・金融的な障壁に関係なく、企業と個人のためのオープンな未来を目指しています。

以下は BTCPay Server の代表的なユースケースです。

- オンラインまたは対面で商品やサービスを販売する **加盟店**
- 自分の資産を守り、資金とフル bitcoin ノードを管理したい自己主権志向の **個人**
- 寄付の受け取りやクラウドファンディングを行いたい **慈善団体・非営利団体**
- bitcoin や最先端の決済インフラ上に構築する **開発者**
- 自分の BTCPay インスタンスに人をオンボードし、決済処理を有償または無償で提供して循環経済を作りたい **ローカルコミュニティ** のメンバー
- BTCPay Server ユーザー向けに即時変換を提供する **取引所**
- BTCPay をクラウドサービスやすぐ使えるハードウェアとして提供する **ホスティング事業者**

![BTCPay UseCase Infographic](./img/infographics/BTCPayUseCasev2-1.png)

本ドキュメントで挙げたグループに、ソフトウェアの利用用途が限定されるわけではありません。

## Merchants

オンラインまたは対面で bitcoin 決済を受け付ける加盟店は、BTCPay Server の主要ユーザー層です。

決済処理に BTCPay Server を選ぶことで、加盟店は次のメリットを得られます。

- コスト削減（BTCPay は無料で、手数料やサブスクリプションが不要）
- 仲介者の排除（セルフホストなら支払いは直接自分のウォレットへ）
- 顧客プライバシーの強化（アドレス再利用なし、セルフホスト時は第三者サーバーへの情報漏えいなし）
- 時間短縮（主要 e-commerce プラットフォームとの簡単な統合）
- ビジネスへの干渉からの保護（自己主権）

### Online stores

インターネットで商品やサービスを販売する加盟店は、通常、主要 e-commerce プラットフォーム向けに提供しているプラグインを利用します。[WooCommerce](WooCommerce.md)、[Shopify](/Shopify.md)、[PrestaShop](/PrestaShop.md)、[Magento](/Magento.md)、[Drupal](/Drupal/)、[Shopaware](https://github.com/lampsolutions/LampSBtcPayShopware) などです。任意の CMS 用プラグインをインストールし、セルフホストの BTCPay または第三者ホストの BTCPay に接続します。

BTCPay Server のチェックアウトは他の決済ゲートウェイと同様です。顧客には請求書が表示されます。顧客は QR コードをスキャンするか、金額と bitcoin アドレスをコピー＆ペーストして支払います。支払いが確認されると、e-commerce ソフトウェア経由で通知を受け取り、商品を発送できます。

### Physical stores

対面販売の小売店向けに、BTCPay Server には [Web ベースの Point of Sale](./Apps.md#point-of-sale-app) があります。オンラインストアと同様に、顧客にはその場で支払える請求書が表示されます。**POS app** は Web 接続可能な任意のデバイスで実行できます。

[デモ POS app](https://mainnet.demo.btcpayserver.org/apps/3utBTfSKkW4gK7aQMd2hW5Bh9Fpa/pos) も確認してください。

## Self-sovereign individuals

**プライバシーを重視する個人** は、秘密鍵を提供せずに日常的な bitcoin 取引のために BTCPay Server の内部ウォレットを利用できます。セルフホスト環境では、[内部ウォレット](./Wallet.md) はフルノードに依存するため、プライバシーが大幅に向上します。[Hardware wallet integration](./HardwareWalletIntegration.md) を使えば、[full node](https://en.bitcoin.it/wiki/Full_node) とハードウェアウォレットを組み合わせ、第三者サーバーへの情報漏えいを防げます。

## Freelancers & bill pay

**フリーランサー** は [Payment Request](./PaymentRequests.md) を共有して支払いを _請求_ できます。Payment Request の内容と見た目はカスタマイズ可能です。有効期限の有無にかかわらず、顧客は任意のタイミングで支払えます。BTCPay Server は、顧客が都合のよいタイミングで支払う際の為替レートを自動更新します。加盟店やフリーランサーは、請求書支払いサービスにも Payment Request を利用できます。友人への送金依頼にも素早く活用できます。

加盟店は [Pull Payment](./PullPayments.md) を共有して支払いを _提供_ することもできます。これは、フリーランサーが都合のよい時に資金を引き出せる長期的な支払いオファーです。加盟店は総額を指定し、部分支払いまたは全額支払いのリクエストを承認できます。

## Charities & non-profits

慈善団体、非営利団体、コンテンツ制作者など、従来の固定 bitcoin アドレス方式よりもプライベートな形で寄付を受け取りたい組織は、[Pay Button](./WhatsNext.md#creating-the-pay-button)、[POS app](./WhatsNext.md#creating-the-point-of-sale-app)、[Crowdfunding app](./Apps.md#crowdfunding-app) を活用して、より良いユーザー体験を提供できます。

寄付受け取りに BTCPay を使う利点:

- コスト削減（手数料・サブスクリプションなし）
- 仲介者の排除（支払いは直接ウォレットへ）
- 団体と寄付者双方のプライバシー向上（アドレス再利用なし、第三者への IP 漏えいなし）

過去には寄付受け取りでアドレスを再利用する人が多くいましたが、BTCPay Server はアドレス再利用を防ぎます。以下は、Bitcoin アドレスを再利用すべきでない理由です。

- プライバシー: 同じアドレスを寄付に再利用すると、本人との結び付けが非常に容易になるだけでなく、寄付者や関係者全員のプライバシーも損ないます
- セキュリティ: プライバシーが損なわれることで攻撃対象領域が広がり、あなたや寄付者に危害を加えたい相手が大量の情報を得られるようになります
- 高額手数料: Bitcoin 取引手数料は送金額ではなく取引の「サイズ」で計算されます。アドレス再利用により多数の入力を含む巨大な取引になり、資金移動時の手数料が高くなります

アドレス再利用の詳細は [Bitcoin Wiki](https://en.bitcoin.it/wiki/Address_reuse) を参照してください。

## Developers

インスタンスをデプロイすると、開発者は Bitcoin 上に構築するためのフル技術スタックを得られます。[Greenfield API](https://docs.btcpayserver.org/API/Greenfield/v1/) を使って構築したり、BTCPay ユーザー向けに無料または有料プラグインを作ったりできます。BTCPay はオープンソース組織なので、[contribute](/Contribute/) に参加し、ソフトウェア改善に貢献することもできます。

## Local communities

BTCPay Server をセルフホストする人は、他ユーザー向けに登録を有効化し、家族・友人・地域コミュニティ向けの [third-party host](/Deployment/ThirdPartyHosting.md) になることができます。これにより、ホストのインスタンスに乗る形で Bitcoin 受け取りを可能にできます。意欲のあるメンバーが地域コミュニティをオンボードし、ローカルでのハイパービットコイニゼーションを促進できます。

[![BTCPay Server for local communities](https://img.youtube.com/vi/9n81qnzlPf8/mqdefault.jpg)](https://www.youtube.com/watch?v=9n81qnzlPf8)

## Cryptocurrency exchanges

BTCPay Server を使う[加盟店数](https://directory.btcpayserver.org)は日々増えており、暗号資産取引所は BTCPay との統合を開発することで恩恵を受けられます。これにより、支払いを法定通貨へ即時変換する機能を提供できます。

## Hosting providers

ホスティング事業者は、顧客向けに簡単な 1-click BTCPay デプロイソリューションを作成できます（すでに提供している事業者もいます）。BTCPay Server への関心が高まる中、ホスティング企業はこの新規顧客層を取り込み、加盟店向けに簡単にデプロイできる BTCPay インスタンスをホストして収益化できます。

---

以上は BTCPay の活用方法の一部です。創造性を発揮し、課題を解決する独自のソリューションを自由に構築してください。
