<!-- legacy-anchor-aliases -->
<span id="configuring-the-gap-limit-in-wasabi"></span>
<!-- /legacy-anchor-aliases -->

# Wasabi Wallet を BTCPay Server に接続する

このドキュメントでは、**[Wasabi Wallet](https://wasabiwallet.io/) を BTCPay Server に接続する方法**を説明します。

1. BTCPay Server で Store を作成する
2. [Wasabi Wallet をダウンロード](https://wasabiwallet.io/#download)
3. [Wasabi Wallet をインストール](https://docs.wasabiwallet.io/using-wasabi/InstallPackage.html)

## Wasabi Wallet セットアップ

インストール後、デスクトップのアイコンをクリックして Wasabi Wallet を開きます。

## クイックセットアップ

1. Wasabi で新しいウォレットを作成する
2. Wasabi の `Wallet Info` で **Extended Account Public Key** をコピーする
3. BTCPay Server で Store > Settings > Wallet > Setup > Connect an existing wallet > Enter extended public key に進む
4. Wasabi の `Receive` で新しいアドレスを生成する
5. Wasabi と BTCPay Server のアドレスが一致することを確認する

## 手順詳細

Wasabi の初回起動時には、`Add wallet` ダイアログが自動的に開きます。
`Create new wallet` を選択して新しいウォレットを生成します。

![Wasabi Add Wallet](./img/Wasabi/WasabiAddWallet.png)

ウォレット名を付けます。例: `BTCPay Server Wallet`。

![Wasabi Add Wallet Name](./img/Wasabi/WasabiAddWalletWalletName.png)

Recovery Words を正しい順序で書き留めてください。

![Wasabi Add Wallet Recovery Words](./img/Wasabi/WasabiAddWalletRecoveryWords.png)

12 個のリカバリーワードのうち 3 つを確認します。
これは正しく書き留めたことを確認するための簡易テストです。

![Wasabi Add Wallet Confirm Recovery Words](./img/Wasabi/WasabiAddWalletConfirmRecoveryWords.png)

パスワードを追加します。
このパスワードはパスフレーズとして使われ、後から変更できません。

:::danger リカバリーワードとパスワードの両方が、このウォレットの復元に必要です
リカバリーワードとパスワードのバックアップを必ず保管してください。
:::

![Wasabi Add Wallet Add Password](./img/Wasabi/WasabiAddWalletAddPassword.png)

**重要:** 画面に表示される順序どおりにリカバリーワードを書き留めてください。紙に書いて安全な場所に保管してください。時間をかけて各単語を三重に確認してください。シードをデジタル形式（写真、テキストファイル）で保存しないでください。シードとパスワードにアクセスできる人は誰でも資金へアクセスできます。Recovery Words と Password は必ず適切にバックアップしてください。

Coinjoin Strategy を選択します。
Wasabi は既定で資金全体を自動的に coinjoin します。
資金を coinjoin したくない場合は、後で Coinjoin Settings の `Automatically start coinjoin` を無効化してください。
coinjoin と関連設定の詳細は [Wasabi Documentation](https://docs.wasabiwallet.io/) を参照してください。

![Wasabi Coinjoin Strategy](./img/Wasabi/WasabiCoinjoinStrategy.png)

ウォレットの作成が完了しました。

![Wasabi Add Wallet Success](./img/Wasabi/WasabiAddWalletSuccess.png)

パスワードを入力して新しいウォレットを開きます。

![Wasabi Open Wallet](./img/Wasabi/WasabiOpenWallet.png)

ウォレットが読み込まれます（時間がかかることがあります）。
読み込み完了後、右上の 3 点メニューをクリックして `Wallet Info` に移動します。

![Wasabi Find Wallet Info](./img/Wasabi/WasabiFindWalletInfo.png)

`Extended Account Public Key` を選択して**コピー**します。これは BTCPay がアドレス導出に使う**公開鍵**です。これを使って秘密鍵を導出したり、bitcoin を使用したりすることはできません。

![Wasabi Extended Account Public Key](./img/Wasabi/WasabiExtendedAccountPublicKey.png)

## ストアウォレットの設定

1. ストアを作成済みで Dashboard にいる前提で、`Set up a wallet` をクリックします。

![Connect Wasabi Wallet to BTCPay Server](./img/createwallet/storedashboard-create.jpg)

2. 上記の手順を wasabi 側で実施したら、`Connect an existing wallet` をクリックします。

![Connect Wasabi Wallet to BTCPay Server](./img/createwallet/storedashboard-connect.jpg)

3. `Enter extended public key` を選択します。

![Connect Wasabi Wallet to BTCPay Server](./img/createwallet/select-xpub.jpg)

4. `Extended Account Public Key` を derivation scheme フィールドへそのまま貼り付け、何も追加せず `Continue` をクリックします。

![Connect Wasabi Wallet to BTCPay Server](./img/createwallet/xpub-form.jpg)

5. Wasabi Wallet に戻り、`Receive` ボタンをクリックして新しいアドレスを生成します。

![Wasabi Receive](./img/Wasabi/WasabiReceive.png)

6. Wasabi Wallet に表示されるアドレスと BTCPay Server に表示されるアドレスを比較し、一致を見つけたら `continue` します。

![Connect Wasabi Wallet to BTCPay Server](./img/createwallet/compare-address.jpg)

7. 一致が見つかれば、ウォレットはストアに接続されています。

![Connect Wasabi Wallet to BTCPay Server](./img/createwallet/wallet-connected.jpg)

### Wasabi を BTCPay Server フルノードに接続する（BTCPay をセルフホストしている場合）

ウォレット接続後は、**Wasabi Wallet を BTCPay のフルノードへ接続する**ことを強く推奨します。手順は簡単ですが、BTCPay をセルフホストし `Admin` でログインしている場合のみ実行できます。BTCPay で Tor を有効化しておく必要があります（既定で有効）。この設定により、さらにプライバシーが向上します。

BTCPay で Server Settings > Services > **Full node P2P > See Information** に進みます。
BTCP-P2P ページで `Show Confidential QR Code` をクリックします。QR Code の下に `See QR Code information by clicking here` というリンクがあるのでクリックして文字列を表示します。文字列をコピーし、`bitcoin-p2p://` の部分を削除します。

Wasabi では `Settings` の Bitcoin タブに移動し、`Bitcoin P2P Endpoint` へそのエンドポイントを貼り付けます。

変更を適用するため、Wasabi を再起動します。

### Wasabi の Gap Limit を設定する

上部の検索バーで `Wallet Folder` をクリックします。しばらくするとサブフォルダ内に `json` ファイルが表示されます。notepad などのテキストエディタでそのファイルを開きます。
`"MinGapLimit": 21,` の行を探し、`"MinGapLimit": 100,` に変更して保存します。

Gap Limit をいくつにすべきかに唯一の正解はありません。多くの加盟店は 100〜200 を設定します。取引量が多い大規模加盟店の場合は、さらに高い値を試せます。

詳細は [Gap Limit, check the FAQ](./FAQ/Wallet.md#missing-payments-in-my-software-or-hardware-wallet) を参照してください。

**Wasabi Wallet と BTCPay Server の接続が完了しました**。BTCPay で受け取った支払いは Wasabi で確認でき、そこで送金や mix を続けて行えます。
