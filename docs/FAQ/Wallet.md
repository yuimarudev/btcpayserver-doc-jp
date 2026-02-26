<!-- legacy-anchor-aliases -->
<span id="can-i-use-a-hardware-wallet-with-btcpay-server"></span>
<span id="do-i-have-to-use-btcpay-server-wallet"></span>
<span id="how-to-add-custom-labels-and-comments-to-transactions"></span>
<span id="how-to-set-up-my-wallet-with-btcpay-server"></span>
<span id="i-dont-see-lightning-network-payments-in-btcpay-wallet"></span>
<span id="is-there-a-mobile-app-for-btcpay-server-wallet"></span>
<span id="what-is-a-derivation-scheme"></span>
<span id="what-is-a-replace-by-fee-rbf-transaction"></span>
<span id="what-is-btcpay-server-wallet"></span>
<span id="what-is-wallet-re-scan-in-btcpay"></span>
<span id="why-is-sending-a-transaction-using-trezor-failing"></span>
<!-- /legacy-anchor-aliases -->

# ウォレットFAQ

このドキュメントには、BTCPay Server の[内部ウォレット](../Wallet.md)に関するよくある質問をまとめています。

[[toc]]

## BTCPay Server ウォレットとは何ですか？

BTCPay Server には内部ウォレットがあり、関連する Bitcoin の入出金トランザクションを確認したり、資金を使用したりできます。

これは他のウォレットと同様に動作しますが、既定でプライバシー機能（ノンカストディアル、第三者不要、サーバー専用フルノードで検証、など）が強化されており、既存ウォレットを BTCPay Server と使う際に発生しうる UX 上の問題も解決します。さらに、トランザクションのカスタムラベル、ブロックチェーンエクスプローラーへのリンク、トランザクション承認ステータスなど、多くの機能も含まれています。さまざまな外部ウォレット型や、サーバー生成のホットウォレットとも接続できます。これらの理由から、BTCPay Server で最も柔軟で優れたウォレット体験のために、内部ウォレットの利用を推奨します。

組み込みウォレットの使い方については、[このページ](../Wallet.md)を確認してください。内部ウォレットを使うには、まず BTCPay ストアで[ウォレットをセットアップ](../WalletSetup.md)する必要があります。

## BTCPay Server でウォレットをセットアップするには？

ストアのウォレットセットアップページでは、BTCPay Server であらゆる種類のウォレットを設定できるよう、手順が段階的に案内されます。さらに質問がある場合は、[ウォレットの設定方法](../WalletSetup.md)に関する詳細ドキュメントを確認してください。

## BTCPay Server でハードウェアウォレットは使えますか？

内部ウォレットには[ハードウェアウォレット統合](../HardwareWalletIntegration.md)が組み込まれています。[BTCPay ウォレット](../Wallet.md)で対応ハードウェアウォレットを使用できます。

これは、ウォレットが BTCPay のフルノードに依存するため、デバイス標準ソフトウェアとは異なり、第三者アプリやサーバーに情報を漏らさずにハードウェアウォレットを使えることを意味します。

## 同じ xpub を使う別ストア間でアドレス再利用は起きますか？

結論から言うと、起きません。
同一インスタンス上の BTCPay Server で、同じ xpub を使って 2 つの別ストアを作成できます。
その場合でも、BTCPay Server は正しくアドレスをローテーションし、ストア間で再利用しません。

:::warning
これは同じインスタンス上で行う必要があります。
[Github issue #960](https://github.com/btcpayserver/btcpayserver-doc/issues/960) に記載があります。
:::

## BTCPay Server ウォレットは必須ですか？

既定では、BTCPay Server が必要とするのは拡張公開鍵だけです。BTCPay ストアで支払いを受け取るには、外部（既存）ウォレットで生成した拡張公開鍵（xPub）を提供します。組み込みウォレットをまったく使わず、代わりに[既存ウォレット](../WalletSetup.md#use-an-existing-wallet)で資金管理することもできます。

ただし、資金管理には組み込みウォレットの利用を推奨します。組み込みウォレットは既定でプライバシーを高めるだけでなく、[gap-limit](#missing-payments-in-my-software-or-hardware-wallet) のようなユーザー体験上の問題も解消します。

## Trezor を使った送金トランザクションが失敗するのはなぜですか？

BTCPay の [HWI (Vault)](../HardwareWalletIntegration.md) と Trezor ウォレットを使って (PSBT) トランザクションを送信しようとした際に（"user refused" や Trezor が反応しないなどの）問題が発生する場合は、Send ページで Advanced Settings を展開し、**Always include non-witness UTXO if available** を有効化してください。

<a id="missing-payments-in-my-software-or-hardware-wallet"></a>
## ソフトウェアまたはハードウェアウォレットで支払いが表示されない

BTCPay Server で[既存のソフトウェアまたはハードウェアウォレット](../WalletSetup.md#use-an-existing-wallet)を使用している場合、BTCPay ウォレット残高と外部ウォレットの Web / デスクトップ / モバイルアプリで表示される残高に差異が出ることがあります。この差異は通常、**gap-limit** の問題に関連しています。

### gap limit の問題

大半のサードパーティウォレットは、複数ユーザーでノードを共有する[ライトウォレット](https://en.bitcoin.it/wiki/Lightweight_node)です。性能上の問題を防ぐため、ライトウォレットもフルノード依存ウォレットも、ブロックチェーン上で追跡する残高ゼロのアドレス数を（通常 20 件に）制限しています。BTCPay Server は請求書ごとに新しいアドレスを生成します。

この前提では、BTCPay Server が未払い請求書を 20 件連続で生成すると、外部ウォレットは新規トランザクションがないと見なして取得を停止します。21 件目、22 件目以降の請求書が支払われても、外部ウォレットには表示されません。

一方、BTCPay Server の内部ウォレットは、自身が生成したあらゆるアドレスを、はるかに大きい gap limit とともに追跡します。第三者に依存しないため、常に正しい残高を表示できます。

### gap limit の解決策

gap limit 問題の解決は簡単ではありません。選択肢は 2 つあります。

1. 既存（外部）ウォレットの gap limit を増やす
2. BTCPay Server の内部ウォレットを使う

#### 1. gap limit を増やす

[外部/既存ウォレット](../WalletSetup.md#use-an-existing-wallet)で gap-limit 設定が可能なら、簡単な対処はその値を増やすことです。ただし、多くのウォレットはこれをサポートしていません。

私たちが把握している、gap-limit 設定が可能なウォレットは次のとおりです。
- [Electrum](../ElectrumWallet.md)
- [Wasabi](../WasabiWallet.md)
- [Sparrow](https://sparrowwallet.com/)
- [Bitcoin Core](https://github.com/bitcoin/bitcoin)
- [Specter](https://specter.solutions/index.html)
- [Nunchuk](https://nunchuk.io/)
- [Samourai Wallet](https://samouraiwallet.com/) (Dojo Maintenance Tool と併用した場合)

残念ながら、それ以外のウォレットでは問題が発生する可能性が高いです。

資金管理に[外部ウォレット](../WalletSetup.md#use-an-existing-wallet)を使いたい場合は、既存ウォレットを次のいずれかへリカバリーし、gap limit を増やすことを推奨します。

- [Electrum で gap limit を増やす](../ElectrumWallet.md#configuring-the-gap-limit-in-electrum)
- [Wasabi で gap limit を増やす](../WasabiWallet.md#configuring-the-gap-limit-in-wasabi)

gap limit を増やした後は、外部ウォレットと BTCPay ウォレットの残高が一致するはずです。一致しない場合は、導出方式の設定が誤っている可能性があります。

#### 2. 内部ウォレットを使う

ユーザー体験とプライバシーを最良にするため、外部ウォレットの使用をやめて [BTCPay Server 内部ウォレット](../Wallet.md)の利用を検討することを推奨します。

## derivation scheme とは何ですか？

BTCPay Server は、[ウォレットの設定方法](../WalletSetup.md)にかかわらず、請求書で受け取る資金の送金先を表すために `derivation scheme` を使用します。これらの資金の送金先は、あなたが提供した拡張公開鍵によって特定されるウォレットです。

拡張公開鍵に異なる導出方式を使用することで、ストア請求書に表示されるさまざまな受取アドレスタイプを選択することもできます。

| アドレスタイプ         |             例             |
| :--------------------- | :------------------------: |
| P2WPKH                 |           xpub...          |
| P2SH-P2WPKH            |       xpub...-[p2sh]       |
| P2PKH                  |      xpub...-[legacy]      |
| マルチシグ P2WSH       |    2-of-xpub1...-xpub2...   |
| マルチシグ P2SH-P2WSH  | 2-of-xpub1...-xpub2...-[p2sh] |
| マルチシグ P2SH        | 2-of-xpub1...-xpub2...-[legacy] |

:::tip
上記の xPub 拡張公開鍵フォーマットに加えて、BTCPay Server は yPub と zPub フォーマットもサポートしています。ウォレットセットアップ完了時にこれらは xPub に変換されますが、資金の受け取りや送金には影響しません。
:::

## Replace-By-Fee (RBF) トランザクションとは何ですか？

Replace-By-Fee (RBF) トランザクションは、Bitcoin プロトコルの機能です。RBF の概要、発生理由、種類については[こちら](https://bitcoin.stackexchange.com/a/54457/85016)を参照してください。

プライバシー強化のため、BTCPay Server 内部ウォレットではトランザクションごとに RBF 機能が既定でランダムに有効/無効になります。常に有効にしたい場合や無効にしたい場合は、BTCPay Server [内部ウォレット](../Wallet.md#rbf-replace-by-fee)の詳細オプションを確認してください。

## BTCPay Server は mempoolfullrbf=1 を使いますか？

非常に短く答えると、はい。
これを BTCPay Server セットアップの既定値として追加することにしました。ただし、無効化できるフラグメントにもしています。
`mempoolfullrbf=1` がない場合、顧客が RBF シグナルなしのトランザクションで二重支払いを行っていたとしても、加盟店がそれを把握できるのは承認後になります。

ただし、このポリシーを有効化したくないユーザーもいます。加盟店のインセンティブには合致する一方で、このポリシーが広く導入されると 0 承認での支払い受け入れが難しくなり、ネットワーク全体の利益には反すると考える人もいます。

無効化するには、次を使用します。

```bash
BTCPAYGEN_EXCLUDE_FRAGMENTS="$BTCPAYGEN_EXCLUDE_FRAGMENTS;opt-mempoolfullrbf"
. btcpay-setup.sh -i
```

## トランザクションにカスタムラベルやコメントを追加するには？

[自動ラベル](../Wallet.md#transaction-labels)に加えて、独自のカスタムトランザクションラベルを簡単に作成できます。ラベルはウォレット表示でトランザクションをフィルタリングするために使えます。支払いに関するメモや説明として、各トランザクションに個別コメントを追加することもできます。

![カスタムトランザクションラベル](../img/wallet/WalletTxComment.png)

## BTCPay ウォレットに Lightning Network の支払いが表示されません

[Lightning Network](../LightningNetwork.md) と BTCPay Server の[ウォレット](../Wallet.md)は別の概念です。BTCPay Server 内部ウォレットにはオンチェーン支払いのみ表示されます。

将来的に統合される可能性はありますが、現時点で Lightning Network の資金を管理するには、Ride the Lightning、ThunderHub、外部接続の Lightning ウォレット（Zeus、Zap など）、またはコマンドラインインターフェース（CLI）を使用してください。

## BTCPay Server ウォレット用のモバイルアプリはありますか？

:::tip
更新 11/2023:
今後、BTCPay Server ウォレット向けモバイルアプリが提供される予定です。[現在開発中です](https://twitter.com/BtcpayServer/status/1699114457421447543)。
:::

BTCPay Server はモバイルアプリではなく Web アプリであり、Web ブラウザを表示できるあらゆるデバイスで利用できます。BTCPay Server の Lightning ノードに接続できるモバイルアプリ（Zeus、Zap など）もあります。

また、P2P または RPC を使ってモバイルアプリから Bitcoin フルノードに接続することもできます。iOS の場合は、Fully Noded を使って簡単に Bitcoin フルノードへ接続できます。

BTCPay ノードを Fully Noded に接続するには:

    1. App Store から Fully Noded をダウンロードします。
    2. BTCPay で Server Settings > Services に移動し、Full Node RPC をクリックします。
    3. Fully Noded アプリを開き、Quick Connect QR を選択します。
    4. BTCPay に表示された QR コードをスキャンします。
    5. これで Bitcoin フルノードが Fully Noded に接続されます。

Fully Noded から簡単に監視できるノード状態やネットワーク情報の一部は次のとおりです。

![Fully Noded](../img/FullyNoded.png)

## BTCPay Server で PSBT（partially signed bitcoin transactions）を使うには？

BTCPay Server を使って PSBT の作成および/またはブロードキャストができます。ガイドとして、[ColdCard ハードウェアウォレットで PSBT トランザクションに署名する](../ColdCardWallet.md#spending-from-btcpay-server-wallet-with-coldcard-psbt) と [Sparrow ウォレットで PSBT トランザクションを作成して署名する](../Sign-PSBT-with-sparrow-wallet.md) を確認してください。
