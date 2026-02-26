# 収益化

収益化機能を使うと、BTCPay Server 管理者は組み込みの [Subscriptions](http://docs.btcpayserver.org/Subscriptions) システムを利用して、自身の BTCPay Server インスタンスのユーザーに課金できます。これは、他者向けに BTCPay Server をホスティングし、サーバー利用料・保守・技術サポート費用を請求したい運用者向けの機能です。

収益化は **BTCPay Server v2.3.0** 以降で利用できます。

[![BTCPay 収益化](https://img.youtube.com/vi/THxH6jRrtgQ/mqdefault.jpg)](https://www.youtube.com/watch?v=THxH6jRrtgQ)

## 収益化の有効化

1. **Server Settings > Maintenance** に移動します  
2. **Monetization** を選択します  
3. **Create a new offering** をクリックします  
4. 資金の入金先となるデフォルトストアを設定します  
5. 価格とトライアル期間を設定します  
6. 必要に応じて、サーバー上の管理者以外の全ユーザーをサブスクリプションモデルへ移行できます  
7. **Proceed** をクリックします  
8. メール SMTP を未設定の場合は、設定を求められます。**Configure Server Email をクリックし、SMTP の詳細を入力してください。**  
9. サブスクリプションプランを後からカスタマイズしたい場合は、右上の **Go to Offering** をクリックします。

![オファー作成](./img/monetization/monetization-1.png)
![オファー作成](./img/monetization/monetization-2.png)
![オファー作成](./img/monetization/monetization-3.png)
![オファー作成](./img/monetization/monetization-4.png)
![オファー作成](./img/monetization/monetization-5.png)
![オファー作成](./img/monetization/monetization-6.png)
![オファー作成](./img/monetization/monetization-7.png)
![オファー作成](./img/monetization/monetization-8.png)

## 収益化はどのように動作しますか？

収益化を有効にすると、BTCPay Server は自動的に次を実行します。

* デフォルトのサブスクリプションオファーとプランを作成（後から設定可能）  
* ユーザーアカウントのサブスクリプションベースアクセス制御を有効化  
* 通常の登録フローをサブスクリプションチェックアウトに置き換え  
* 基本的なメール通知を事前設定（サーバー SMTP が設定済みの場合）

以後、BTCPay Server に登録するユーザーは最初にサブスクリプションチェックアウトを行う必要があります。フローは次のとおりです。

* ユーザーが BTCPay Server に登録する  
* ユーザーは Subscription Checkout に進み、プラン選択とメールアドレス入力を行う  
* 登録情報がメールで送信される

![オファー作成](./img/monetization/monetization-9.png)

## ユーザーアクセスと請求

* 有効なサブスクリプションがあると BTCPay Server アカウントにアクセスできます  
* サブスクリプションの期限切れまたは停止中はアクセスできません  
* アカウントアクセスが無効でも、ユーザーは請求ポータルにアクセスして更新やチャージを行えます  
* 定期支払いをリマインドする自動メールがユーザーに送信されます  
* ユーザーはアカウント内の **Manage billing** ページ、または自動メール内のリンクから請求情報を管理します

![オファー作成](./img/monetization/monetization-10.png)

この一連のフローは高度にカスタマイズ可能です。事前設定の範囲を超える設定を行いたい場合は、[subscription documentation](http://docs.btcpayserver.org/Subscriptions) を確認してください。

## 注意事項と制限

* 収益ベースの請求はまだ利用できません  
* 高度な権限コントロールは将来のリリースで予定されています  
* この機能はベータ版です。今後の改善のため、ぜひフィードバックをお寄せください
