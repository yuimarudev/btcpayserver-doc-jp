# Google Cloud デプロイ

このセットアップは [Docker Deployment](https://docs.btcpayserver.org/Docker/) とほぼ同じですが、`docker-compose` を **Google Cloud** 上でホストする点が異なります。

## Google Cloud shell セットアップ

Google Cloud は BTCPayServer をセットアップするための別の方法です。

まず、次のボタンをクリックしてください。

[![Open in Cloud Shell](https://gstatic.com/cloudssh/images/open-btn.svg)](https://console.cloud.google.com/cloudshell/open?git_repo=https%3A%2F%2Fgithub.com%2Fbtcpayserver%2Fbtcpayserver-googlecloud&page=editor)

Google アカウントで [Google Cloud Console](https://console.cloud.google.com) にログインできます。

インストールの最終手順:

- Google cloud shell で、インスタンスをデプロイするデフォルトの project と zone を設定する
- yaml ファイルを変更して VM インスタンスと BTCPay server を設定する: ![GCE and BTCPay Config](../img/gcloud-yaml.png)
- シェルスクリプトのモードを 755 に変更し、`deploy.sh <any deployname>` を実行してデプロイを開始する
- （Google Cloud のデプロイが完了するまで1分ほど待つ）
- Google cloud shell に固定 IP が表示される
- DNS サービスでドメイン名（例: EXAMPLE.MYSITE.com）にマッピングする
- Google cloud console の VM instances 一覧から vm に ssh する
- ssh 後、`/btcpayserver-docker` ディレクトリへ移動して `changedomain.sh EXAMPLE.MYSITE.com` を実行する
- ブラウザで https://EXAMPLE.MYSITE.com にアクセスする
- `Register` をクリックしてアカウントを作成する（これが **admin** アカウントになります）
- **完了!** `https://EXAMPLE.MYSITE.com/stores` にアクセスしてストアを作成し、請求を開始してください。

上級ユーザーは `https://EXAMPLE.MYSITE.com/server/services/ssh` の情報を使って SSH 接続し、次を実行できます。

- `docker ps` と `docker logs xxx` で実行中プロセスを確認する
- `btcpay-down.sh` と `btcpay-up.sh` で BTCPayServer を停止・起動する

概算コスト: **月額 70 USD**

詳細: [btcpayserver/btcpayserver-googlecloud](https://github.com/btcpayserver/btcpayserver-googlecloud)
