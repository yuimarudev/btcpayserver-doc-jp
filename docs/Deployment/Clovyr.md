# Clovyr BTCPay Server Web デプロイ

このガイドでは、Clovyr BTCPay Server Web デプロイの初期セットアップを順に説明します。
Clovyr は、BTCPay Server を含むさまざまなサービスを素早く簡単にデプロイできます。デプロイ先として、Clovyr 自身のサーバーだけでなく、Digital Ocean、AWS、Linode など複数の VPS プロバイダーも選択できます。

## 1. Clovyr BTCPay Server 起動画面にアクセス

[Clovyr BTCPay Server launch page](https://clovyr.app/apps/btcpayserver/launch) にアクセスします。

![Clovyr Launch](../img/Clovyr/1.png)

次に、ホスティング先を選択します。ここではデフォルトの Clovyr 自社サーバーを選びます。
Review をクリックし、その後 Launch をクリックします。

## 2. アカウント作成

Clovyr ダッシュボード内に成功画面が表示されます。

![Clovyr Dashboard](../img/Clovyr/2.png)

Create account をクリックして、必要情報を入力します。

![Clovyr Dashboard](../img/Clovyr/3.png)
パスワードを忘れたり失ったりした場合にアカウントを復元できる Recovery kit の保存を求められます。

![Clovyr Dashboard](../img/Clovyr/4.png)

## 3. BTCPay Server へアクセス

Clovyr ダッシュボードに戻ったら、App Essentials の下にある "Open" ボタンをクリックすると、BTCPay Server が新しいタブで開きます。

## 4. 最初のストアを始める

新しい BTCPay Server で最初のアカウント作成を求められます。Administrator account にチェックが入っていることを確認してください。

これで最初のストアをセットアップする準備ができました。
ストア設定の詳細は、この [Guide](../RegisterAccount.md) を参照してください。

## 5. BTCPay Server ダッシュボードへようこそ

新しい BTCPay Server にログインできました。
まだ bitcoin ウォレットは設定されていません。[このウォレットセットアップガイド](../WalletSetup.md)を参照してください。

:::tip
ノード、デプロイ、アップデートについて質問がある場合は、Clovyr の Feedback タブから Clovyr サポートにお問い合わせください。
:::

## 6. 請求

サービスは 7 日間の無料トライアルがあります。その後は月額 $20 が課金され、Clovyr ダッシュボードで "Upgrade now" をクリックして設定が必要です。
