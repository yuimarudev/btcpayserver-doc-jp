# Web デプロイメントの概要

このページでは、利用可能な `Web Deployments` を紹介します。BTCPay Server をサービスとして提供する事業者が増えたら、この一覧も更新していきます。
では、`Web deployment` とは何でしょうか。
簡単に言うと、サードパーティがホストする環境です。

何をしてくれるのでしょうか。利用者にストレージ容量と計算リソースを提供します。
特にこのページで扱うのは、ホスト型の Bitcoin ノードです。

これで、自分でハードウェアを購入・ホスト・保守する必要はありません。これらはサービス提供者が担います。
その一方で、自分の bitcoin ノードとその上で動く BTCPay Server のホスティングを他者に任せるという信頼リスクがあります。
各ホスティング事業者にはそれぞれ長所と短所があります。価格が安いところもあれば、サポート品質が高いところもあります。
ここでは、掲載している各サービスと BTCPay Server ソリューションを順に案内します。

以下では、各サービスの簡単な紹介と個別の詳細ガイドを確認できます。

## 既知の Web デプロイメント

### LunaNode web-wizard

[LunaNode](https://www.lunanode.com/) はカナダ拠点のホスティング事業者で、Bitcoin 支払いに対応しており、本人確認は電話番号認証のみで利用できます。

同社の web wizard は、**非常に使いやすい UI から BTCPay Server をデプロイする最も簡単な方法の1つ**です。
LunaNode は、開始時にサーバー用の汎用ドメインを提供します。

`LunaNode` の詳細は [こちら](./LunaNode.md)

### Voltage Cloud

[Voltage](https://www.voltage.cloud) は Bitcoin 向けのインフラプロバイダーです。
Bitcoin に特化しているため、顧客に高品質なサービスを提供できます。
Voltage の Lightning ノードを自動接続できます。

`Voltage Cloud` の詳細は [こちら](./voltagecloud.md)

### Clovyr

[Clovyr](https://clovyr.app/) はアプリケーションデプロイサービスです。
BTCPay Server を含む各種サービスを、自社サーバーまたは Digital Ocean、AWS、Linode など複数の VPS 事業者へデプロイできます。
`Clovyr` の詳細は [こちら](./Clovyr.md)

## Elestio

[Elestio](https://elest.io/) は、コードやオープンソースソフトウェアをデプロイするためのフルマネージド DevOps プラットフォームです。 
Digital Ocean、AWS、Linode など多数のクラウドサービスプロバイダーから選択できます。Elestio は移行、バックアップ、バージョン変更、セキュリティ対応を担い、BTCPay Server を簡単にデプロイできるよう支援します。

`Elestio` の詳細は [こちら](https://elest.io/open-source/btcpay)

## デプロイメントが不足していませんか？

私たちは FOSS プロジェクトのため、取りこぼしているデプロイメントがあるかもしれません。
未掲載のデプロイメントを見つけた場合、追加してほしい・通知したい場合はぜひ知らせてください。
[Mattermost app](https://mattermost.com/download/) をダウンロードして Mattermost の [community chat](https://chat.btcpayserver.org/) に参加するか、[Telegram](https://t.me/btcpayserver) で連絡できます。
また、BTCPay server Documents の [Github](https://github.com/btcpayserver/btcpayserver-doc/issues) で issue を作成することもできます。
