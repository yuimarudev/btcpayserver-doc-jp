<!-- legacy-anchor-aliases -->
<span id="rbf-replace-by-fee"></span>
<span id="signing-with-a-hot-wallet"></span>
<span id="signing-with-hd-private-key-or-mnemonic-seed"></span>
<!-- /legacy-anchor-aliases -->

# BTCPay Server ウォレット

BTCPay Server には、**フルノードに依存した内蔵ウォレット**があり、資金を簡単に管理できます。

各 [store](./CreateStore.md) で設定された暗号通貨ごとに、メニューバーの Wallets 配下へ個別ウォレットが表示されます。

## ウォレット機能

ウォレットには次の機能があります。

1. Transactions
2. Send
3. Receive
4. Rescan
5. Pull payments
6. Payouts
7. PSBT
8. Settings

### Transactions

着金（緑）、送金（赤）、未承認（グレー）の**トランザクション**が、タイムスタンプと残高とともに日付順で表示されます。トランザクション ID をクリックすると、ブロックエクスプローラーで詳細を確認できます。

![Individual Wallet](./img/wallet/WalletTransactions.png)

#### Transaction Labels

以下の表は、**BTCPay が使用する各種トランザクションラベル**を示しています。

| Transaction Type | Description |
| ---------------- | ----------- |
| app              | アプリ作成の請求書経由で支払いを受領 |
| invoice          | 請求書経由で支払いを受領 |
| payjoin          | 未払い。請求書タイマーはまだ期限切れではない |
| payjoin-exposed  | 請求書の payjoin 提案を通じて UTXO が露出 |
| payment-request  | 支払いリクエスト経由で支払いを受領 |
| payout           | 出金または返金で支払いを送信 |

独自の [custom transaction labels and comments](./FAQ/Wallet.md#how-to-add-custom-labels-and-comments-to-transactions) も作成できます。

### Send

Send 機能では、**BTCPay ウォレット内の資金を使用**できます。

![Send from the Wallet](./img/wallet/WalletSend.png)

#### Advanced Settings

一部のウォレット機能は上級ユーザー向けです。`Send` タブ内の `Advanced Settings` を有効化すると確認できます。

##### UTXO のおつりを作成しない

このオプションは `Send` ページの `Advanced mode` で利用できます。

これはプライバシー強化機能で、別の自分のウォレットや取引所へ送金する際に有用です。送金額を**切り上げる**ことで、おつり UTXO が作られないようにします。

既定ではこの機能は無効です。たとえばウォレットに `1.1 BTC` の UTXO があり、`1.0 BTC` を入力すると、結果のトランザクションは `0.1 BTC` のおつりと、宛先への `1.0 BTC` の 2 出力になります。

ブロックチェーン分析では、その `0.1 BTC` のおつりが以前 `1.1 BTC` を管理していた同一主体に属すると判断でき、同様のパターンで将来の支出を追跡される可能性があります。

この機能を有効にすると、BTCPay Server ウォレットは送金額を `1.1 BTC` に切り上げ、おつり出力を自分に返さないようにします。

警告: この例では金額欄に `1.0` と入力していても、実際に宛先へ送られる金額は `1.1 BTC` になります。

##### その他の機能

###### カメラ QR スキャン

ウォレットのスキャン機能（送金画面のカメラアイコン）を使うと、**アドレスまたは BIP21 支払いリンクを含む QR コードを端末カメラで読み取れます**。送金情報が自動入力されるため、アドレスと金額を手動でコピー＆ペーストする必要がありません。

###### BIP21 アドレスを貼り付け

このオプションは **BIP21 支払いリンクをデコード**します。[Payjoin](./Payjoin.md) 請求書を支払う際に便利です。

#### トランザクションへの署名（支出）

資金を使うには、トランザクションに**署名**する必要があります。署名は次の方法で行えます。

- Hardware Wallet
- PSBT をサポートするウォレット
- HD 秘密鍵またはリカバリーシード
- Hot Wallet

##### HD 秘密鍵またはニーモニックシードで署名

[既存ウォレットを BTCPay Server に設定](./WalletSetup.md#use-an-existing-wallet)した場合、適切な入力欄へ秘密鍵を入力して資金を使えます。Wallet > Settings で `AccountKeyPath` を正しく設定しないと支出できません。

##### PSBT 対応ウォレットで署名

PSBT（**Partially Signed Bitcoin Transactions**）は、まだ完全に署名されていない Bitcoin トランザクションの交換フォーマットです。
PSBT は BTCPay Server でサポートされており、互換性のあるハードウェア/ソフトウェアウォレットで署名できます。

完全署名済み Bitcoin トランザクションの作成は、次の手順で進みます。

- 特定の入力と出力を持つ PSBT を作成（署名なし）
- エクスポートした PSBT を対応ウォレットへインポート
- ウォレットでトランザクションデータを確認して署名
- 署名済み PSBT ファイルをウォレットからエクスポートし、BTCPay Server へインポート
- BTCPay Server が最終的な Bitcoin トランザクションを生成
- 結果を検証してネットワークへブロードキャスト

チュートリアル:
- [Sign a PSBT transaction with ColdCard Hardware Wallet](./ColdCardWallet.md#spending-from-btcpay-server-wallet-with-coldcard-psbt)（完全オフライン/エアギャップ）
- [Create and sign a PSBT transaction with Sparrow wallet](./Sign-PSBT-with-sparrow-wallet.md)

##### ハードウェアウォレットで署名

BTCPay Server はハードウェアウォレットを内蔵サポートしており、第三者アプリやサーバーに情報を漏らさず **BTCPay でハードウェアウォレットを使用**できます。

[手順](HardwareWalletIntegration.md)を確認して、[互換性のあるハードウェアウォレット](https://github.com/bitcoin-core/HWI#device-support)で設定・署名してください。

##### ホットウォレットで署名

ストア設定時に [新規ウォレットを作成](./CreateWallet.md)し、それを [hot wallet](./CreateWallet.md#hot-wallet) として有効化した場合、バージョン 1.2.0 以降では、[hot wallet](./CreateWallet.md#hot-wallet) 作成時にサーバーへ保存されたシードを自動使用して署名するオプションが追加されています。

:::danger
ホットウォレット機能にはセキュリティ上の影響があります。必ず [Hot Wallet documentation](./CreateWallet.md#security-implications) を読み、内容を理解してください。
:::

### Receive

Receive タブでは、**支払い受け取りに使える未使用アドレス**を生成します。同じことは請求書の作成（Invoices > Create new invoice）でも行えます。

![Wallet Receive](./img/wallet/WalletReceive.png)

![Wallet Receive Two](./img/wallet/WalletReceiveTwo.png)

### Pull Payments

この機能を使うと、**Pull Payment** を作成でき、外部の個人があなたのウォレットから資金を `pull` できるようリクエストできます。

詳細は [Pull Payments](./PullPayments.md) を参照してください。

### Payouts

このセクションでは Pull Payments を管理し、**外部の個人から要求された出金を承認または拒否**できます。

詳細は [Payouts](./PullPayments.md#approve-and-pay-a-payout) を参照してください。

### Settings

`wallet` の右上に `wallet settings` があります。
wallet settings タブでは、各種設定を調整できます。[新しいウォレットを作成](./CreateWallet.md)した場合、または [hardware wallet integration](./HardwareWalletIntegration.md) で既存ウォレットを使用した場合、これらの設定は事前構成されています。
ここでは、欠落トランザクションの再スキャン、古いトランザクションの剪定、ウォレットフレーズの表示、ウォレット削除など、複数の操作を実行できます。

![Wallet Rescan](./img/wallet/WalletSetting.png)

外部ウォレットの拡張公開鍵を手動で追加した場合、BTCPay Wallet から支出するには、外部ウォレットで確認できる `AccountKeyPath`（例: `m/84'/0'/0'`）に調整する必要があります。

`wallet settings` には、該当ストア用の `speed policy` もあります。
`Payment` 配下には主に 2 つの設定があり、[Payment invalid if transaction fails to confirm in ... after invoice creation](./FAQ/Stores.md#payment-invalid-if-transactions-fails-to-confirm--minutes-after-invoice-expiration) と [Consider the invoice confirmed when the payment transaction...](./FAQ/Stores.md#consider-the-invoice-confirmed-when-the-payment-transaction) です。後者では、決済完了と見なすのに必要な承認数を設定できます。

![Wallet settings](./img/wallet/WalletSettingTwo.png)

### Re-scan

Rescan は Bitcoin Core 0.17.0 の `scantxoutset` を使って、設定された導出スキームに属するコインを**ブロックチェーンの現在状態**（UTXO Set）からスキャンします。

![Wallet Rescan](./img/wallet/WalletRescan.png)

ウォレット再スキャンは、BTCPay ユーザーにとって重要な 2 つの問題を解決します。

1. [Gap limit](./FAQ/Wallet.md#missing-payments-in-my-software-or-hardware-wallet)
2. 過去に使用されたウォレットのインポート

**Gap limit**: 多くのウォレットはアドレスのギャップリミットが 20 に設定されています。つまり、加盟店が連続して 21 件以上の未払い請求書を受け取ると、それらのウォレットで残高表示が不正確になり、一部トランザクションが見えなくなる場合があります。

**Wallet import**: 過去に取引履歴のあるウォレット（既使用ウォレット）の導出スキームを追加した場合、BTCPay では過去の残高とトランザクションを表示できません。

![Wallet rescan progress](./img/wallet/WalletRescanProgress.png)

Re-scan はこの両方を解決する機能です。スキャン完了後、BTCPay Server は過去のトランザクションを含めて正しい残高を表示します。

ウォレット再スキャンにはフルノードへのアクセスが必要なため、この機能はサーバー所有者のみ利用できます。

第三者ホストを利用しているユーザーは、新しく生成した xpub キーを使い、ギャップリミットを増やせる Electrum などの外部ウォレットを併用してください。

### Labels

wallet settings の最下部で、`custom transaction label` を管理できます。

リンクをクリックすると、すべてのトランザクションに関連付けられたカスタムラベル一覧ページへ移動します。必要な権限があれば、任意またはすべてのカスタムラベルを削除できます。

![Wallet settings](./img/wallet/ManageLabel.png)
