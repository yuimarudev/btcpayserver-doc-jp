<!-- legacy-anchor-aliases -->
<span id="connect-an-existing-wallet"></span>
<!-- /legacy-anchor-aliases -->

## 既存のウォレットを接続する

![Import Existing Wallet](./img/createwallet/ImportWallet.png)

既存ウォレットを使うと、BTCPay Server にウォレットの秘密鍵を渡さずに外部ウォレットで支払いを受け取れます。悪意ある攻撃者がサーバーを侵害して xpub を取得した場合、取引履歴は観測できても資金にはアクセスできません。

- [ハードウェアウォレットを接続する（推奨）](#connect-hardware-wallet)
- [ウォレットファイルをインポートする（推奨）](#import-wallet-file)
- [拡張公開鍵を入力する](#enter-extended-public-key)
- [ウォレットの QR コードをスキャンする](#scan-wallet-qr-code)
- [ウォレットシードを入力する（非推奨）](#enter-wallet-seed)

<a id="connect-hardware-wallet"></a>

### ハードウェアウォレットを接続する

ハードウェアウォレットは、安全性と使いやすさのバランスが優れています。すでにセットアップ済みなら、BTCPay Server と簡単に連携できます。組み込みの [hardware wallet integration](HardwareWalletIntegration.md) により、ハードウェアウォレットの xpub キーが自動で BTCPay Server に追加されます。この連携により、BTCPay の [internal wallet](./Wallet.md) 内でストアに入金された資金を利用することもできます。

:::tip
ハードウェアウォレットをお持ちの場合は、[既存のハードウェアウォレットを BTCPay Server で使う手順](HardwareWalletIntegration.md) に従ってください。
:::

<a id="import-wallet-file"></a>

### ウォレットファイルをインポートする

既存のソフトウェアウォレットを使う方法は、外部ウォレットがすでに作成・バックアップ済みであることを前提とします。理論上は拡張公開鍵を提供できるモバイル/デスクトップウォレットなら利用できますが、多くのウォレットには技術的制約（[(gap-limit)](./FAQ/Wallet.md#missing-payments-in-my-software-or-hardware-wallet)）があり、後で深刻なユーザー体験上の問題になる可能性があります。

そのため、以下に挙げるソフトウェアウォレットのみの使用を推奨します。

- [Electrum Wallet](./ElectrumWallet.md)
- [Wasabi Wallet](./WasabiWallet.md)

上記リンク先には、それぞれのソフトウェアウォレットを BTCPay Server と連携する手順がステップごとに記載されています。

外部ソフトウェアウォレットで受け取った資金を使う・管理するには、[internal BTCPay Wallet](./Wallet.md) でトランザクションを作成し、秘密鍵で署名する方法か、外部ウォレット側でそのまま管理する方法が使えます。

<a id="enter-extended-public-key"></a>

### 拡張公開鍵を入力する

この方法は、[レガシーウォレットアドレス](./FAQ/General.md#what-if-i-have-a-problem-paying-an-invoice) を調整したい場合や、ウォレット種別が Hardware Wallet Integration (Vault) に対応していない場合に有用です。

この方法ではウォレット接続を手動で設定する必要があり、ウォレットの拡張公開鍵、アカウントキーパス、マスターフィンガープリントについて十分な理解がある場合にのみ使用してください。

<a id="scan-wallet-qr-code"></a>

### ウォレットの QR コードをスキャンする

一部のウォレットでは、ウォレットを作成して拡張公開鍵（xPub）を QR コードでエクスポートできます。スキャン QR コードオプションを使うことで、これらのウォレットを BTCPay Server に簡単に接続できます。ただし、ウォレット提供者側に調整手段がない限り、一般的な [(gap-limit)](./FAQ/Wallet.md#missing-payments-in-my-software-or-hardware-wallet) の問題はどの xPub でも発生する可能性があります。

[internal BTCPay Wallet](./Wallet.md) で資金を使って管理するには、トランザクション署名時に（xpub QR コード生成に使った）秘密鍵が必要です。あるいは BTCPay で受け取った資金を外部ウォレット側で管理することもできます。

<a id="enter-wallet-seed"></a>

### ウォレットシードを入力する

この方法は、特定のウォレットで資金を使う手段が**他にない場合**に有効です。例えば、以前はハードウェアウォレット連携に対応していたが現在は非対応のアルトコインウォレットなどです。一般に、インターネット接続されたデバイスへウォレットシード語を入力するべきではありません。

この方法ではウォレット接続を手動で設定する必要があり、ウォレット形式、拡張公開鍵、アカウントキーパス、マスターフィンガープリントについて十分な理解がある場合にのみ使用してください。
