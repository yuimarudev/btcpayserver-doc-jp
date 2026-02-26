# Invoice Ninja で Bitcoin 決済を受け付けるには？

![BTCPay Server Invoice](./img/InvoiceNinja/BTCPayInvoiceNinja.png)

[Invoice Ninja](http://invoiceninja.com) は、小規模事業者、フリーランサー、起業家が請求書・支払い・顧客を管理するための高機能な請求/課金ソフトウェアです。豊富な機能により、請求業務を簡素化し、ユーザーが本業の成長に集中できるようにします。

[BTCPay Server](https://btcpayserver.org) と Invoice Ninja を連携すると、手数料や仲介者なしで Bitcoin 決済を受け付けられ、支払いは直接あなたの Bitcoin ウォレットに入ります。

このガイドでは、BTCPay Server を InvoiceNinja で設定して使う手順を説明します。BTCPay の決済機能は InvoiceNinja に直接統合されているため、プラグインは不要です。

[![Invoice Ninja](https://img.youtube.com/vi/4oy-DCf6lhw/mqdefault.jpg)](https://www.youtube.com/watch?v=4oy-DCf6lhw)


## 要件

- InvoiceNinja（ホスト版またはセルフホスト版）
- BTCPay Server（[self-hosted](https://docs.btcpayserver.org/Deployment/) または [third-party provider](https://docs.btcpayserver.org/Deployment/ThirdPartyHosting/) によるホスト）
- BTCPay Server 上で [ストア作成済み](https://docs.btcpayserver.org/CreateStore/)
- BTCPay Server 上で [ウォレット接続済み](https://docs.btcpayserver.org/WalletSetup/)

## 1. Payment Gateway の設定

Invoice Ninja で BTCPay を設定するには、次の手順に従ってください。

1. Invoice Ninja の管理ポータルにログインします。
2. Settings > **Payment Settings** に移動します。
3. 右上の **Add payment gateway** をクリックします。
4. ドロップダウンをスクロールして **BTCPay** をクリックします。
5. 次に BTCPay 設定ページが表示されます。**認証情報を入力する前に**、まず **Save をクリックして payment gateway を作成することが重要です**。

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja1.png)

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja2.png)

payment gateway を作成したら、手順 2 に進み、BTCPay Server と Invoice Ninja をペアリングします。

## 2. BTCPay Server をペアリングする

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja3.png)

### 2.1 BTCPay URL

`BTCPay URL` フィールドには、セルフホストサーバーの URL か [third-party provider](https://directory.btcpayserver.org/filter/hosts) の URL をそのまま入力します。
*例: https://mainnet.demo.btcpayserver.org*

### 2.2 BTCPay Store ID

BTCPay Store ID は BTCPay Server の **Store Settings > General > Store ID** で取得できます。コピーして `BTCPay Store ID` フィールドに貼り付けます。

### 2.3 API キーを生成する

BTCPay API キーを作成するには、サイドバー下部の Account をクリックします。
1. **Manage Account > API Key** をクリックします。
2. **Generate API** key ボタンをクリックします。
3. チェックボックスで以下の権限を有効にします。
    - View invoices
    - Create an invoice
    - Modify invoices
    - Modify selected stores' webhooks
    - View your stores
    - Create non-approved pull payments in selected stores
4. 必要に応じて、複数の BTCPay ストアがある場合は権限を適用するストアを選択します。
5. **API キーを表示してコピー**し、Invoice Ninja の `API Key` フィールドに**貼り付け**ます。

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja4.png)

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja5.png)

### 2.4 Webhook を生成する

1. InvoiceNinja の **Payment Settings > Edit Payment Gateway** で **Payment Gateway** タブを開き、**Webhook URL** をコピーします。
2. 次に BTCPay Server の Store Settings > **Webhooks** に移動します。
3. **Create Webhook** ボタンをクリックします。
4. InvoiceNinja でコピーした Webhook URL（手順1）を **Payload URL** フィールドに貼り付けます。
4. Secret フィールド横の "Eye" アイコンをクリックしてシークレットキーを表示し、コピーします。
5. 変更を反映するため、`Add webhook` をクリックするのを忘れないでください。
6. InvoiceNinja に戻り、Webhook Secret フィールドに Secret Key を貼り付けます。
7. **Save** をクリックして変更を反映します。

*"Settings" タブと "Limits/Fees" タブでは、他の InvoiceNinja 決済方式と共通の追加オプションを設定できます。*

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja6.png)


### 3. Crypto 決済を有効化する

すべて設定できたら、Payment Gateway Settings タブで Crypto 支払いオプションを有効化し、保存してください。

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja7.png)

これで完了です。請求書を発行して Bitcoin で支払いを受け取れます。

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja8.png)

![BTCPay Server Invoice](./img/InvoiceNinja/InvoiceNinja9.png)
