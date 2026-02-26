# Refunds

:::tip
加盟店から返金を受ける手順を知りたい場合は、この [FAQ](./FAQ/General.md#what-if-i-have-a-problem-with-a-paid-invoice) を参照してください。
:::

**Refunds** は [Pull Payments](./PullPayments.md) 機能を基盤にしたアプリケーションの 1 つです。

このページでは、返金を発行する手順を説明します。  
顧客向け返金を作成するには、いくつかの短い手順があります。

## 返金を作成する

1. 請求書を返金するには、`Invoices` ページで対象請求書の `Details` をクリックします。

![BTCPay Server refund feature](./img/refunds/invoices-details.jpg)

2. `Issue a refund` をクリックします。

![BTCPay Server refund feature](./img/refunds/issue-refund.jpg)

3. 返金の支払い方法を選択します。

![BTCPay Server refund feature](./img/refunds/issue-refund-payment-option.jpg)

4. 返金する `amount` を選択します。

![BTCPay Server refund feature](./img/refunds/issue-refund-amount.jpg)

5. このページのリンクを顧客に共有します。

![BTCPay Server refund feature](./img/refunds/claimingside.jpg)

## 返金を処理する

顧客が提供したリンクを開き、返金先 bitcoin アドレスを追加して請求すると、次は返金処理です。

1. サイドバーの `Payouts` タブへ移動します。

![BTCPay Server Payouts tab](./img/refunds/payouts-status3-options-appr.jpg)

2. 処理したい Payouts を選択し、actions から `Approve and send` を選びます。

![BTCPay Server Payouts tab](./img/refunds/payouts-status3-options-appr.jpg)

3. トランザクションに署名してブロードキャストします。

![BTCPay Server Payouts tab](./img/refunds/payouts-status4-options-sign3-adv.jpg)

4. payout は署名済みとなり、ブロックチェーン上の確認待ちになります。この状態は申請者側の画面にも反映されます。

![BTCPay Server Payouts tab](./img/refunds/payout-status-succesfull.jpg)

5. トランザクションがブロックチェーンで承認されると、payout のステータスは `completed` になります。

![BTCPay Server Payouts tab](./img/refunds/payouts-status5-completed1.jpg)

返金処理が成功した後の顧客側画面。

![BTCPay Server Payouts tab](./img/refunds/claiment-completed1.jpg)
