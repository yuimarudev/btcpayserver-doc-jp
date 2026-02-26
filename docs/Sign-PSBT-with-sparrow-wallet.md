# BTCPay Server と Sparrow wallet で PSBT を作成する

このガイドでは、BTCPay Server で部分署名済み Bitcoin トランザクション（PSBT）を作成し、[Sparrow wallet](https://www.sparrowwallet.com/) で署名してブロードキャストする方法を説明します。この例では [BitBox02](https://bitbox.swiss/) ハードウェアウォレットを使用しますが、サポートされている他のハードウェアウォレットでも動作します。エアギャップ環境を使っている場合や、トランザクション作成者と署名者が別の人である場合に役立ちます。

[[toc]]

## 1. トランザクションの作成（BTCPay Server 側）:

* BTCPay Server にログインし、送金元にしたいストアを選択します
* "Wallets" で "Bitcoin" を選択します
* **[Send]** ボタンをクリックします

### 送金画面での操作:

![BTCPay: Create transaction on BTCPay Server](./img/psbt-send-sparrow/btcpay-1-send.png "Create a transaction")

* 送金先の bitcoin アドレスを入力します
* 金額を入力します
* 任意: 手数料率を変更します（トランザクションをどの程度早く承認したいかに応じて、現在の手数料率は [mempool.space](https://mempool.space) で確認できます）
* **重要**: "Advanced Settings" をクリックして展開し、"**Always include non-witness UTXO if available**" を確認します（BitBox02 のようなハードウェアウォレットが署名するために必要です）
* 任意: "Allow fee increase (RBF)" を "Yes" に設定します（手数料を低くしすぎた場合に、手数料を引き上げてトランザクションの滞留を防げます）
* **[Sign transaction]** ボタンをクリックします

### 署名方法の選択画面:

![BTCPay: Choose signing method: Partially Signed Bitcoin Transaction](./img/psbt-send-sparrow/btcpay-2-choose-signing-method.png "Select Partially Signed Bitcoin Transaction")

* "Partially Signed Bitcoin Transaction" を選択します

### PSBT 画面:

![BTCPay: Download the PSBT file](./img/psbt-send-sparrow/btcpay-3-download-psbt.png "PSBT screen overview, download PSBT")

* "Export PSBT for signing" のアコーディオンを開き、**[Download PSBT file]** ボタンをクリックします
* ファイルをハードドライブに保存します（自分で PSBT に署名することも、Sparrow wallet 側で署名する人に送ることもできます。以下参照）。例: psbt-export.psbt


## 2. PSBT の署名と送信（Sparrow wallet 側）

* Sparrow wallet アプリを開き、ストアで使用している xPub のデータを保持している対応ウォレットを開きます
* 次に、BTCPay Server で作成した PSBT ファイルをインポートします
* メニューで: File -> Open Transaction -> File...
* 保存したファイル（または経理担当から送られたファイル）を選択します。例: psbt-export.psbt

### インポートした PSBT トランザクション表示画面:

![Sparrow wallet: Load the PSBT file](./img/psbt-send-sparrow/sparrow-1-loaded-psbt-for-signing.png "Loaded PSBT for signing")

* 見出し "Signatures:" の下で、"signing wallet" が送金元にしたいウォレットと一致していることを確認します
* **[Finalize Transaction for Signing]** をクリックします

### トランザクションの署名:

![Sparrow wallet: PSBT loaded, ready for signing](./img/psbt-send-sparrow/sparrow-2-loaded-psbt-sign.png)

* **[Sign]** をクリックします

### ハードウェアウォレット接続ポップアップ:

![Sparrow wallet: connect to hardware wallet (e.g. in our case BitBox02)](./img/psbt-send-sparrow/sparrow-3-scan-for-hww.png "Connect your hardware wallet")

* ハードウェアウォレットを接続します（この例では BitBox02）
* ハードウェアウォレットの PIN を入力します（BitBox02 の場合は BitBox アプリを開く案内が画面に出ますが、開く必要はなく、PIN 入力可能になるまで待ちます）
* BitBox02 のロック解除後、**[Scan...]** をクリックするとハードウェアウォレットが表示されます

### ウォレット接続成功:

![Sparrow wallet: hardware wallet successfully connected](./img/psbt-send-sparrow/sparrow-4-unlocked-hww.png "BitBox02 successfully connected")

* **[Sign]** をクリックします
* トランザクション概要が BitBox02 デバイスに表示されるので、デバイス側で確認してください

### トランザクションのブロードキャスト:

![Sparrow wallet: broadcast the transaction](./img/psbt-send-sparrow/sparrow-5-broadcast-transaction.png "Broadcast the transaction")

* 署名が成功したら、トランザクションを Bitcoin ネットワークへブロードキャストする必要があります
* **[Broadcast Transaction]** をクリックします

:::tip
別の方法として、Sparrow wallet からブロードキャストせずに（例: エアギャップ環境の場合）、テキストボックスから署名済みトランザクション PSBT をコピーして BTCPay Server にアップロードし、BTCPay Server 側でネットワークへブロードキャストすることもできます。
:::

**お疲れさまでした。完了です！**
