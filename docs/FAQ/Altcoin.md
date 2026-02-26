<!-- legacy-anchor-aliases -->
<span id="can-an-xyz-coin-be-added-in-btcpay"></span>
<span id="how-to-add-an-altcoin-in-btcpay"></span>
<span id="how-to-add-an-altcoin-to-an-existing-btcpay-deployment"></span>
<span id="how-to-remove-a-coin-from-btcpay"></span>
<span id="which-coins-does-btcpay-server-support"></span>
<!-- /legacy-anchor-aliases -->

# アルトコイン FAQ

このページでは、代替暗号通貨（アルトコイン）に関するよくある質問に回答します。

[[toc]]

## BTCPay Server はどのコインをサポートしていますか？

Bitcoin はこのプロジェクトおよびコア開発者の唯一の注力対象です。ただし、複数のアルトコイン向けにオプトインの統合が用意されています。

- BGold (BTG)（Bitcoin Gold としても知られています）
- Bitcore (BTX)
- Dash (DASH)
- Dogecoin (DOGE)
- Feathercoin (FTC)
- Groestlcoin (GRS)
- Liquid Bitcoin (LBTC)（Liquid Tether USDt をサポート）[(デプロイと利用に関する注記)](https://github.com/btcpayserver/btcpayserver/issues/1282)
- Litecoin (LTC)
- Monacoin (MONA)
- Monero (XMR) [プラグイン経由](https://plugin-builder.btcpayserver.org/public/plugins/monero-plugin) [(デプロイと利用ガイド)](https://sethforprivacy.com/guides/accepting-monero-via-btcpay-server/)
- Tron 上の USDt (Tether) [プラグイン経由](https://plugin-builder.btcpayserver.org/public/plugins/tether-usdt)。
- Viacoin (VIA)
- Z-Cash (ZEC) [プラグイン経由](https://plugin-builder.btcpayserver.org/public/plugins/zcash-plugin)

アルトコインはそれぞれのコミュニティによって保守されており、ここでの掲載は利便性のためのものです。アルトコインのデプロイ、機能、不具合に関するサポートは、各アルトコインのメンテナーまたはコミュニティに直接お問い合わせください。

## 交換プラグイン経由でのアルトコイン受け入れ

以下のプラグインを使うと、ストアウォレット経由で Bitcoin に交換することで、さまざまなアルトコインを受け取れます。

- [Exolix](https://plugin-builder.btcpayserver.org/public/plugins/exolix-plugin)
- [Fixed Float](https://plugin-builder.btcpayserver.org/public/plugins/fixed-float)
- [SideShift](https://plugin-builder.btcpayserver.org/public/plugins/sideshift)
- [Trocador](https://plugin-builder.btcpayserver.org/public/plugins/trocador-app)

これらの外部交換プロバイダーは、ユーザーに手数料を課し、取引ごとの最小額を設定している場合があります。

## BTCPay に XYZ コインを追加できますか？

いいえ。BTCPay 開発者は、リクエストに応じて代替コインを追加しません。新しいコインの追加は、そのコインのコミュニティと開発者に明確に依存します。さらに、BTCPay 開発者はアルトコインの過度なテストや保守を行いません。新しいコインの PR を提出する場合は、動作することを確認してください。アルトコイン統合がアクティブに保守されていない場合、BTCPay から削除されます。

## BTCPay にアルトコインを追加するには？

BTCPay に新しいコインを追加するには、[こちらの手順](../Development/Altcoins.md)に従ってください。

## 既存の BTCPay デプロイにアルトコインを追加するには？

既存の BTCPay Server インストールでコイン数を増やす場合は、マシンに十分なストレージ容量があることを確認してください。

この例では Bitcoin のみが有効で、Docker デプロイに Litecoin を追加します。

コイン構成:

```
BTCPAYGEN_CRYPTO1: First supported cryptocurrency (e.g., BTC, LTC. Default: btc)
BTCPAYGEN_CRYPTO2: Second supported crypto currency (e.g. btc, ltc. Default: (empty))
BTCPAYGEN_CRYPTON: N'th supported crypto currency where N is 9 at maximum. (eg. btc, ltc. Default: (empty))
```

2 番目のコイン（CRYPTO2）として Litecoin を追加するには、次を実行します。

```bash
sudo su -
export BTCPAYGEN_CRYPTO2="ltc"
. ./btcpay-setup.sh -i
```

## BTCPay からコインを削除するには？

[上の例](#how-to-add-an-altcoin-to-an-existing-btcpay-deployment)では、2 番目のコインとして Litecoin を追加しました。特定のコインを削除するには、次のコマンドを使います。

```bash
sudo su -
export BTCPAYGEN_CRYPTO2=""
. ./btcpay-setup.sh -i
```

削除したいコイン番号に応じて、CRYPTO**2** を置き換えてください。たとえば `BTCPAYGEN_CRYPTO3` に XYZ コインがあり、それを削除したい場合は CRYPTO**3** を使います。
