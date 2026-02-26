# Azure デプロイ

このセットアップは [Docker Deployment](https://docs.btcpayserver.org/Docker/) と似ていますが、`docker-compose` を **Microsoft Azure** 上でホスティングする点が異なります。

## ワンクリックセットアップ

まず、次のボタンをクリックします。

[![Deploy to Azure](../img/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbtcpayserver%2Fbtcpayserver-azure%2Fmaster%2Fazuredeploy.json)

[Azure](https://azure.microsoft.com/en-us/account/) には Microsoft アカウントでログインできます。

最終インストール手順:

残りのオプションを入力します: ![Azure Resource Config](../img/AzureResourceConfig.png)

- 確認のため `'Purchase'` をクリック
- （Azure のデプロイ完了を待つ）
- 検索バーに `ip` と入力し、最初の項目 `BTCPayServerPublicIP` を選択
- `DNS name` の下にある Azure デプロイのホスト名をコピー: ![Azure BTCPayServerPublicIP](../img/AzureBTCPayServerPublicIP.png)
- そのホスト名へアクセス（主要ブラウザ対応）
- `'Register'` をクリックしてアカウントを作成。これが **admin** アカウントになります
- ドメインレジストラでこのホスト名にドメインを向ける（詳細: [How to change your BTCPay Server domain name](../FAQ/Deployment.md#how-to-change-your-btcpay-server-domain-name)）
- その後、`https://EXAMPLE.eastus.cloudapp.azure.com/server/maintenance` にアクセス
- あなたのドメイン名を入力して `'Confirm'` をクリック
- （1〜5 分待機）
- **完了!** `https://EXAMPLE.MYSITE.com/stores` にアクセスしてストアを作成し、請求を開始できます。

上級者は、`https://EXAMPLE.MYSITE.com/server/services/ssh` の情報で SSH 接続し、次を実行できます。

- 実行中プロセス確認のため `docker ps` と `docker logs xxx` を実行
- BTCPayServer を停止・起動するため `btcpay-down.sh` と `btcpay-up.sh` を実行

概算コスト（unpruned、Bitcoin-only、Azure の 200 ドル無料トライアル後）: **月額 60 USD**

すべてのノード同期が完了し、動作確認できたら、節約のために[このガイド](./AzurePennyPinching.md)で調整してください。コストは **月額 30〜40 USD** まで下げられるはずです。

[![BTCPay Server - Azure](https://img.youtube.com/vi/xh3Eac66qc4/mqdefault.jpg)](https://www.youtube.com/watch?v=xh3Eac66qc4)

詳しくは: [btcpayserver/btcpayserver-azure](https://github.com/btcpayserver/btcpayserver-azure)
