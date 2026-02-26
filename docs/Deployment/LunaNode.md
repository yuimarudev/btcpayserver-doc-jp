# BTCPay のデプロイ - LunaNode Web-Wizard

この記事では、**LunaNode Web-Wizard を使った BTCPay Server のデプロイ**を説明します。[LunaNode](https://www.lunanode.com/) はカナダ拠点のホスティングプロバイダーで、Bitcoin 支払いに対応しており、本人確認は電話番号認証のみ必要です。

この Web-Wizard は、**非常に使いやすいインターフェースから BTCPay Server をデプロイできる**最も簡単な方法の 1 つです。技術的な知識があまりない場合はこの方法を強く推奨します。LunaNode は開始時にサーバー用の汎用ドメインを提供します。カスタムドメインを設定したい場合は、コマンドラインインターフェースにある程度慣れている必要があります。

月額およそ US$15.80 で、Bitcoin フルノードと Lightning Network ノードを含む **セルフホスト型 BTCPay Server** を運用できます。

以下の BTC Sessions の動画では、BTCPay Server のデプロイとセットアップ手順、さらに Lightning Network を複数の方法で設定する手順を説明しています。

[![How To Accept Bitcoin Payments by deploying BTCpay Server via Web-Deployment](https://img.youtube.com/vi/-GJr4XjRCPo/mqdefault.jpg)](https://www.youtube.com/watch?v=-GJr4XjRCPo)

また、デプロイ手順のみを短くまとめた内容は [こちらの記事](https://medium.com/@BtcpayServer/launch-btcpay-server-via-web-interface-and-deploy-full-bitcoin-node-lnd-in-less-than-a-minute-dc8bc6f06a3) でも確認できます。

## 1. アカウントを作成してクレジットを追加する

LunaNode に登録し、アカウントにクレジットを追加します。
請求書の確認が完了するまで待つ必要がある点に注意してください。

## 2. API キーを作成する

アカウントの確認が完了し、クレジットを追加したら、API セクションに移動して新しい API を作成します。そのページは閉じずに、手順 3 に進んでください。

## 3. Web-Wizard でデプロイする

1. [launchbtcpay.lunanode.com](https://launchbtcpay.lunanode.com/) にアクセスします。
2. 手順 2 で作成した API Key と API ID を貼り付けて続行します。
3. 独自ドメインまたは LunaNode が自動生成したドメインを使用します。
4. 必要に応じて Web-Wizard の設定をカスタマイズします。
5. Launch VM をクリックします。仮想マシンのデプロイには 6〜7 分かかります。

カスタムドメインを使用した場合は、

6. LunaNode が生成したパスワード、または公開鍵・秘密鍵ペアを使って VM に SSH 接続します。
7. 次のコマンドを実行します。

```bash
$ sudo su -
$ export BTCPAY_HOST=your.cool.domain
$ export BTCPAY_PROTOCOL=https
$ export REVERSEPROXY_DEFAULT_HOST="$BTCPAY_HOST"
$ cd btcpayserver-docker
$ . btcpay-setup.sh -i
$ . btcpay-restart.sh -i
```

8. ドメインにアクセスし、アカウントを作成してログインします。

次に、ブロックチェーンの同期完了まで待つ必要があります。使用するプランと追加したコイン数によって、1〜7 日かかる場合があります。Bitcoin と LND で CPU 使用率を有効にすると、1〜2 日で完了します。CPU 使用率を有効にすると高速同期のために US$3 の一時料金がかかります。ノードの同期が完了すると同期ポップアップは消えます。

## 4. 追加のカスタマイズ（任意）

BTCPay Server インスタンスのセットアップ後は、他のデプロイ方法と同様に、LND の keysend や autopilot の有効化など環境変数を追加できます。
詳細は利用可能な [環境変数一覧](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を参照してください。これには [サーバーへ SSH 接続する方法](/FAQ/ServerSettings.md#how-to-ssh-into-my-btcpay-running-on-vps) の知識が必要です。
