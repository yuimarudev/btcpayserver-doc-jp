# Cal.com の予約で Bitcoin 決済を受け付ける

すべての [Cal.com](https://cal.com/) の予約やアポイントメントで、Bitcoin 決済を受け付けられるようになりました。
コンサルティングサービスでも 1 対 1 のミーティングでも、Bitcoin をウォレットへ直接受け取れます。仲介者なし、プラットフォーム手数料なし、隠れたコストもありません。

[![Cal.com](https://img.youtube.com/vi/uttN0YILu3s/mqdefault.jpg)](https://www.youtube.com/watch?v=uttN0YILu3s)

## 前提条件:

セットアップを始める前に、以下を準備してください。

- [Cal.com account](https://cal.com/)
- BTCPay Server - [self-hosted](/Deployment/) または [third-party host](/Deployment/ThirdPartyHosting.md) で稼働
- [Created BTCPay Server store](CreateStore.md) と [wallet set up](WalletSetup.md)


## Cal.com を BTCPay Server と接続する

Cal.com アカウントにログインし、`Apps` > `App store` > `Payments apps` に移動します。

`BTCPayServer` アプリを見つけて `Details` をクリックし、続けて `Install App` ボタンをクリックします。

![Cal.com: image 1](./img/calcom/1_app_install_details.png)

接続したい BTCPay Server インスタンスのアプリケーションを選択します。

![Cal.com: image 2](./img/calcom/2_installation_step_one.png)


次のステップでは BTCPay の認証情報を入力します。新しいタブで BTCPay Server インスタンスを開いてください。

**BTCPay Server URL**: BTCPay インスタンスの URL（例: https://example.btcpay.com）

**BTCPay Store Id**: Cal.com に接続したいストアです。BTCPay Server インスタンスで対象ストアを選び、左側ナビゲーションの `Settings` をクリックすると `storeId` が表示されます。

Store Id をコピーして、Cal.com の BTCPay Server インストールフォームに入力します。

**API Key**: BTCPay で `Account` > `Manage Account` > `API Keys` に移動します。

`Generate Key` をクリックして新しい API キーを作成します。ラベル欄には例として `BTCPay-Calcom` などの名前を付けてください。

権限は次を選択してください:
- View Invoice (btcpay.store.canviewinvoices)
- Create Invoice (btcpay.store.cancreateinvoice)
- Modify store webhook (btcpay.store.webhooks.canmodifywebhooks)

完了したら `Save` をクリックします。API キーをコピーし、Cal.com のインストール画面でフォーム入力を完了してください。

3 つの項目をすべて入力したら、connect ボタンをクリックしてインストールを完了します。すべての値が検証されると、
キーが保存され、Cal.com ページへリダイレクトされます。

**注記** このインストール手順では、BTCPay Server に webhook が作成されます。


![Cal.com: image 3](./img/calcom/3_installation_step_two.png)


![Cal.com: image 4](./img/calcom/4_btcpay_apikey.png)


## デモ

イベントタイプページで任意の予約を選択し、Edit をクリックします。

Cal.com ではイベントごとに個別設定されるため、すべてのイベントで Bitcoin 決済を受け付けたい場合は、各イベントで手動で有効化する必要があります。

![Cal.com: image 6](./img/calcom/6_event_types_booking.png)

編集ページでメニューから Apps を選択し、BTCPay Server アプリケーションを見つけて有効化します。

希望する通貨を選び、金額を指定します。完了したら save をクリックします。

![Cal.com: image 7](./img/calcom/7_event_payment_booking_setup.png)


イベントリンクをコピーして新しいタブで開きます。日付と時間を選び、`Pay to book` ボタンをクリックします。

次のページで BTCPay Server の請求書を支払います。請求書は iFrame 内に表示されます。表示が小さい場合は、
請求書ページの下に新しいタブで開くボタンがあるので、それをクリックして支払いを完了してください。


支払いが完了すると、ミーティングが予約済みであることを示すページへリダイレクトされます。

これで予約に対して Bitcoin 決済を受け付けられます。


![Cal.com: image 8](./img/calcom/8_booking_flow_1.png)


![Cal.com: image 9](./img/calcom/9_booking_flow_2.png)


![Cal.com: image 10](./img/calcom/10_booking_flow_3.png)


![Cal.com: image 12](./img/calcom/12_booking_flow_5.png)


![Cal.com: image payment](./img/calcom/Invoice_payment.png)


![Cal.com: image 13](./img/calcom/13_booking_flow_6.png)


![Cal.com: image 14](./img/calcom/14_booking_flow_7.png)



## サポートとコミュニティ

サポートが必要な場合や追加の質問がある場合は、[Mattermost](https://chat.btcpayserver.org/) または [Telegram](https://t.me/btcpayserver) のサポートチャンネルに参加してください。
