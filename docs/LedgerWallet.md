<!-- legacy-anchor-aliases -->
<span id="quick-setup"></span>
<!-- /legacy-anchor-aliases -->

# Ledger Wallet を BTCPay Server に接続する

このドキュメントでは、**Ledger Nano S Wallet を BTCPay Server に接続する方法**を説明します。

:::warning
Ledger Nano S の直接連携は**現在サポートされていません**。Bitcoin ウォレットについては、[新しいハードウェアウォレット連携](./HardwareWalletIntegration.md) を使って通常どおり Ledger ハードウェアウォレットを利用できます。

[altcoin](/Development/Altcoins.md) ウォレットでは、外部ウォレットから資金を使うか、[内部ウォレット](./Wallet.md) 内で [HD Private Key or mnemonic seed](./Wallet.md#signing-with-hd-private-key-or-mnemonic-seed) または [hot wallet](./Wallet.md#signing-with-a-hot-wallet) を使ってトランザクション署名できます。

新しい altcoin ウォレットを設定する場合は、拡張公開鍵を手動追加するか、[新しいウォレットを作成](./CreateWallet.md) してください。
:::

## Ledger Nano S Wallet セットアップ

このガイドは Nano S ウォレットの初期設定が済んでいる前提です。Nano S の設定は [メーカーサイトのクイックセットアップガイド](https://www.ledger.com/start/) を参照してください。

### 要件

1. Ledger に Bitcoin App がインストール済み
2. Google Chrome または Firefox
3. Firefox の場合、about:config で U2F を有効化
4. PC に他の U2F デバイス（Yubikey、他ウォレットなど）を接続していないこと

### クイックセットアップ

1. Ledger Nano S を PC に接続します。
2. Ledger 上で Bitcoin app を開きます。
3. BTCPay Server で Store > Settings > Wallet > Setup > Derivation Scheme > Import from Hardware Device > Ledger wallet へ進みます。
4. 使用するアカウントを選択します。ほとんどの場合 `Account 0` です。
5. ウォレット上で `Export public key` を確認します。
6. 拡張公開鍵が Ledger から BTCPay Server ストアへ自動追加されます。
7. derivation scheme が `Enabled` になっていることを確認します。
8. `Continue` をクリックします。
9. BTCPay でアドレス一致を `Confirm` します。

これで Ledger ウォレットが BTCPay に接続されます。支払いは Ledger に直接入ります。

#### 手動セットアップ

Ledger に 20 を超えるアカウントがある場合、選択リストの最大表示数（20）により正しいアカウントが見つからないことがあります。
その場合、次の手順で希望アカウントの拡張公開鍵を手動取得できます。

1. [Ledger live app](https://shop.ledger.com/pages/ledger-live) を開く
2. Accounts -> 対象アカウントを選択
3. 右上のツールアイコンから Edit Account
4. Edit Account -> ADVANCED LOGS
5. 拡張公開鍵文字列をコピー
6. "DerivationScheme" テキストフィールドに手動で貼り付け
7. [上記クイックセットアップの手順7](#quick-setup) 以降を続行

![Ledger Account "Advanced Logs" info screenshot](./img/LedgerHelpXpub.png)

### Ledger で BTCPay Server ウォレットから送金する

Ledger 接続済み BTCPay Wallet に資金が入ったら、ハードウェアウォレットでトランザクション署名して送金できます。これにより、第三者サーバーへ情報を漏らさずに、Ledger と自分のフルノードを簡単に連携できます。

1. Ledger Nano S を PC に接続します。
2. Ledger 上で Bitcoin app を開きます。
3. BTCPay で Wallets > Manage > Send へ進みます。
4. 宛先アドレスと金額を入力します。
5. `your Ledger Wallet device` で署名をクリックします。
6. BTCPay が Ledger と接続し、ウォレット画面にトランザクション情報を表示します。
7. Ledger 上でトランザクションを確認します。
8. Ledger で `Ready To Sign` をクリックします。
9. トランザクションを確認し、`Broadcast` をクリックしてネットワークへ送信します。

以下の動画では、BTCPay ストアを Ledger に接続する方法と、[内部 BTCPay ウォレット](./Wallet.md) で Ledger を使う方法を紹介しています。

[![BTCPay Server and Ledger](https://img.youtube.com/vi/1Sj5mP4TkFI/mqdefault.jpg)](https://www.youtube.com/watch?v=1Sj5mP4TkFI)
