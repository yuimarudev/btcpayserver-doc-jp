# Testnet デモ

まず、新しいストアを作成します。

1. [Testnet website](https://testnet.demo.btcpayserver.org/) にアクセスします。
2. 右側の **Create an account** をクリックして [アカウントを作成](https://testnet.demo.btcpayserver.org/register) するか、すでにアカウントがある場合は **Sign In** をクリックします。
3. サインイン後、新しいストアを作成します。

次に、Electrum を使ってストア用の testnet ウォレットを作成します。

1. [Electrum](https://electrum.org) をダウンロードします。
2. `--testnet` パラメータ付きで Electrum を起動します（例: Mac OS では `open -a Electrum.app --args --testnet`）。
3. ウィザードを進め、Electrum が提案するデフォルト設定でテスト用ウォレットを作成します。
4. ウォレット作成後、Electrum メニューの「Wallet」>「Information」を開きます。
5. 「Master Public Key」の文字列（`*pub...` で始まる）をコピーします。

続いて、ストアで Electrum ウォレットを使うように設定します。

1. BTCPay のストア設定ページを開きます。
2. 「General Settings」ページにある「Wallet」セクションで、「Setup」ボタンをクリックしてオンチェーンウォレットを設定します。
3. Electrum からコピーした「Master Public Key」を「Derivation Scheme」テキストフィールドに貼り付け、「Continue」をクリックします。
4. Electrum の「Receive」を開いてアドレスを確認します。「Receiving address」が BTCPay に表示される最初のアドレスと一致しているはずです。
5. 設定後、BTCPay アカウントの [Wallets page](https://testnet.demo.btcpayserver.org/wallets) にテストウォレットが表示されます。

その後、請求書は次のいずれかで作成できます。

- Web サイトの「Invoice」メニュー
- [Custom integration](../CustomIntegration.md) に記載された手順

## 質問

Testnet 上の BTCPay Server について質問がある場合は、[community chat](https://chat.btcpayserver.org/) に参加してください。
その他のツールやコマンドなどについては、インターネット検索や [StackOverflow](https://stackoverflow.com/) で解決できる可能性が高いです。
