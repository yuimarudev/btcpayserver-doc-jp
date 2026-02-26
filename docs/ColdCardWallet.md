<!-- legacy-anchor-aliases -->
<span id="spending-from-btcpay-server-wallet-with-coldcard-psbt"></span>
<!-- /legacy-anchor-aliases -->

# Coldcard Wallet を BTCPay Server に接続する

このドキュメントでは、**Coldcard Wallet** を BTCPay Server で使う方法を説明します。

## Coldcard Wallet のセットアップ

このガイドは、Coldcard ウォレットのセットアップが完了していることを前提としています。**Coldcard** の設定は、[メーカーサイトのクイックセットアップガイド](https://coldcardwallet.com/docs/quick)を参照してください。

### クイックセットアップ

1. MicroSD カードを Coldcard ウォレットに挿入します。
2. Advanced > MicroSD Card > Electrum Wallet > Native-Segwit を開きます。
3. MicroSD カードを PC に戻します。
4. BTCPay Server で、Stores > Settings > Setup > Connect an existing wallet > `Import wallet file` を開きます。
5. Choose File から、先ほど Coldcard でエクスポートしたウォレットファイルを選択します。
6. `Continue` をクリックします。
7. BTCPay Server に表示されるアドレスと一致していることを確認します。

これで **Coldcard は BTCPay Server に接続されました**。支払いは Coldcard に直接入金されます。以下の動画で、BTCPay ストアを Coldcard に接続する流れを確認できます。

[![BTCPay and Coldcard](https://img.youtube.com/vi/N0eVwdP_7EQ/mqdefault.jpg)](https://www.youtube.com/watch?v=N0eVwdP_7EQ)

### Coldcard を使って BTCPay Server ウォレットから支出する（PSBT）

**Coldcard に接続された BTCPay Wallet** に資金が入金されたら、[PSBT](https://github.com/bitcoin/bitcoin/blob/master/doc/psbt.md#psbt-in-general)（Partially Signed Bitcoin Transactions）を使って支出できます。これにより、ハードウェアウォレットをインターネットに接続せず、完全オフラインでトランザクション署名が可能です。

1. Wallets > Manage > Send
2. 送金先アドレスと金額を入力します。
3. `a wallet supporting PSBT` で署名するボタンをクリックします。
4. PSBT タブに遷移し、情報が事前入力された状態になるので、`Sign with a wallet supporting PSBT (save as file)` をクリックします。
5. ファイルを MicroSD カードに保存します。
6. MicroSD を Coldcard に挿入します。
7. Coldcard で `Ready To Sign` をクリックします。
8. トランザクション内容を確認し、OK ボタンで署名します。
9. 署名済みトランザクションが MicroSD に保存されます。
10. BTCPay のウォレット PSBT タブで、署名済み PSBT ファイルをアップロードします。
11. `Decode` をクリックします。
12. `Other Actions` から `Review` を選択します。
13. トランザクションを確認し、`Broadcast` をクリックしてネットワークに送信します。

以下の動画では、**BTCPay ストアを Coldcard に接続する方法**を紹介しています。

[![BTCPay Server and Coldcard](https://img.youtube.com/vi/XyqvYaXMfNU/mqdefault.jpg)](https://youtu.be/XyqvYaXMfNU)
