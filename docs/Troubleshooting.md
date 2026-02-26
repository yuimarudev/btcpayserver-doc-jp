<!-- legacy-anchor-aliases -->
<span id="13-restarting-a-service"></span>
<span id="21-btcpay-logs"></span>
<span id="23-bitcoin-node-logs"></span>
<!-- /legacy-anchor-aliases -->

# BTCPay Server の問題をトラブルシューティングする

問題が起きるのは決して楽しいことではありません。このドキュメントでは、**問題を特定する**ために役立つ最も一般的な手順を説明し、可能であれば自分で、またはコミュニティの助けを借りて解決できるようにします。

問題の特定は非常に重要です。

## 1. 問題を再現する

まず最初に、問題がいつ発生するのかを確認してください。
問題を再現できるか試してください。
サーバーを更新して再起動し、同じ問題を再現できるか確認してください。
問題の説明に役立つと思う場合は、スクリーンショットを撮ってください。

### 1.1 サーバーを更新する

[BTCPay のバージョン](./FAQ/ServerSettings.md#how-can-i-see-my-btcpay-version)を確認してください。
現在のバージョンが BTCPay の[最新バージョン](https://github.com/btcpayserver/btcpayserver/releases)よりかなり古い場合、[サーバーの更新](./FAQ/ServerSettings.md#how-to-update-btcpay-server)で問題が解決する可能性があります。

### 1.2 サーバーを再起動する

サーバーの再起動は、多くの一般的な BTCPay Server の問題を簡単に解決できる方法です。
再起動するには、[サーバーに SSH 接続](./FAQ/ServerSettings.md#how-to-ssh-into-my-btcpay-running-on-vps)する必要がある場合があります。

### 1.3 サービスを再起動する

問題によっては、BTCPay Server デプロイ内の特定サービスだけを再起動すれば解決する場合があります。
たとえば、SSL 証明書更新のために letsencrypt コンテナを再起動します。

```bash
sudo su -
cd btcpayserver-docker
docker restart letsencrypt-nginx-proxy-companion
```

別のサービスを再起動したい場合は、`docker ps` でサービス名を確認してください。

<a id="2-looking-through-the-logs"></a>
## 2. ログを確認する

ログには、問題解決に不可欠な情報が含まれていることがあります。
以下では、**BTCPay の各コンポーネントのログ情報**を取得する方法を説明します。

### 2.1 BTCPay ログ

v1.0.3.8 以降、BTCPay Server のログをフロントエンドから簡単に確認できます。
サーバー管理者であれば、**Server Settings > Logs** に移動してログファイルを開いてください。
ログ内の特定エラーの意味がわからない場合は、トラブルシューティング時に必ずその内容を共有してください。

さらに詳細なログが必要で、Docker デプロイを使っている場合は、コマンドラインで個別の Docker コンテナのログを確認できます。
VPS 上で動作している BTCPay インスタンスに [SSH 接続する手順](./FAQ/ServerSettings.md#how-to-ssh-into-my-btcpay-running-on-vps)を参照してください。

以下は、BTCPay でよく使われるコンテナ名の一覧です。

| ログ対象 |            コンテナ名             |
| -------- | :-------------------------------: |
| BTCPayServer |     generated_btcpayserver_1      |
| NBXplorer    |       generated_nbxplorer_1       |
| Bitcoind     |       btcpayserver_bitcoind       |
| Postgres     |       generated_postgres_1        |
| proxy        | letsencrypt-nginx-proxy-companion |
| Nginx        |             nginx-gen             |
| Nginx        |               nginx               |
| Core Lightning (CLN)  |  btcpayserver_clightning_bitcoin  |
| LND          |     btcpayserver_lnd_bitcoin      |
| RTL          |    generated_lnd_bitcoin_rtl_1    |
| Thunderhub   |     generated_bitcoin_thub_1      |
| LibrePatron  |            librepatron            |
| Tor          |              tor-gen              |
| Tor          |                tor                |

以下のコマンドで、コンテナ名を指定してログを表示できます。
他のコンテナログを見たい場合は、コンテナ名を置き換えてください。

```bash
sudo su -
cd btcpayserver-docker
docker ps
docker logs --tail 100 generated_btcpayserver_1
```

### 2.2 Lightning Network ログ

Lightning Network で問題がある場合は、以下を利用してください。

### 2.2.1 - Lightning Network LND - Docker

Docker 利用時に LND ログへアクセスする方法はいくつかあります。
まず root としてログインします。

`sudo su -`

正しいディレクトリへ移動します。

`cd btcpayserver-docker`

コンテナ名を確認します。

`docker ps`

コンテナ名でログを表示します。

`docker logs --tail 100 btcpayserver_lnd_bitcoin`

あるいは、コンテナ ID（先頭の一意な数文字だけで可）でも簡単に表示できます。

`docker logs 'add your container ID '`

何らかの理由でさらに多くのログが必要な場合

`sudo su -`

`cd /var/lib/docker/volumes/generated_lnd_bitcoin_datadir/_data/logs/bitcoin/mainnet/`

そのディレクトリ内で `ls` を実行します。

`lnd.log  lnd.log.13  lnd.log.15  lnd.log.16.gz  lnd.log.17.gz` のようなファイルが表示されます。

未圧縮ログを読むには `cat lnd.log`、別のログなら `cat lnd.log.15` を使います。

`.gzip` の圧縮ログを読むには `gzip -d lnd.log.16.gz` を使います（この例では `lnd.log.16.gz`）。

これで新しいファイルが生成され、`cat lnd.log.16` で表示できます。

上記でうまくいかない場合は、先に gzip をインストールする必要があるかもしれません: `sudo apt-get install gzip`

### 2.2.2 - Lightning Network Core Lightning (CLN) - Docker

`sudo su -`

`docker ps`

Core Lightning (CLN) コンテナ ID を見つけます。

docker logs 'add your container ID here'

または、次のコマンドを使います。

`docker logs --tail 100 btcpayserver_clightning_bitcoin`

Core Lightning (CLN) の cli コマンドでもログ情報を取得できます。

`bitcoin-lightning-cli.sh getlog`

## 2.3 - Bitcoin Node ログ

Bitcoind コンテナの[ログ確認](#2-looking-through-the-logs)に加えて、[bitcoin-cli コマンド](https://developer.bitcoin.org/reference/rpc/index.html)を使って bitcoin ノード情報を取得することもできます。
BTCPay には、Bitcoin ノードと簡単に通信できるスクリプトが含まれています。

`btcpayserver-docker` フォルダ内で、ノードからブロックチェーン情報を取得します。

`bitcoin-cli.sh getblockchaininfo`

## 3. 自分で解決策を探す（Google、FAQ、GitHub issues）

環境は異なっていても、同じ問題を他の誰かがすでに経験している可能性は高いです。
少し時間を取って検索し、自分で解決できるか確認してみてください。

### 3.1 BTCPay FAQ

よくある問題は [Frequently Asked Questions ページ](./FAQ/README.md)に記録するようにしています。
質問が載っているか確認してください。

### 3.2 GitHub

高度な技術的問題がある場合、ユーザーは GitHub に issue を作成することが多いです。
BTCPay の GitHub リポジトリで [クローズ済み issue を検索](https://github.com/btcpayserver/btcpayserver/issues?q=is%3Aissue+is%3Aclosed)してみてください。

### 3.3 Mattermost

Mattermost チャットは、他のユーザーが以前に経験した類似問題を探すのに適しています。
右上の検索をクリックし、クエリを入力してください。

## 4. 助けを求める

自分で問題を解決できなくても心配いりません。
助けてくれる活発なコミュニティがあります。

問題をより正確に説明するほど、迅速に解決できる可能性が高くなります。
簡潔に、かつ関連情報をできるだけ多く提供してください。
[利用中のバージョン](./FAQ/ServerSettings.md#how-can-i-see-my-btcpay-version)と BTCPay のデプロイ構成は必ず含めてください。
何をしようとしていて、何が問題なのかを説明してください。
可能であればログを添付してください。
関連があるなら、スクリーンショットもぜひ追加してください。

以下は、質問の良い例です。

> XYZ で問題が発生しています。問題は再現できます。BTCPay バージョンは 0.100.31 で、Docker deployment guide に従って Digital Ocean にサーバーをデプロイしました。FAQ とクローズ済み GitHub issues を調べましたが解決策が見つかりません。私の BTCPay セットアップは XYZ で、XYZ を実行すると問題が発生します。BTCPay インスタンスから取得できたログは以下です。添付画像にエラーが表示されています。

:::warning 注意:
コミュニティはカスタムデプロイに対して広範なサポートを提供しません。
つまり、[Manual Deployments](/Deployment/ManualDeployment.md) のような構成は、開発目的および **デプロイとメンテナンスの問題を自力で解決できる** 技術的知識を持つユーザー向けです。これには [Hardware-As-A-Service](/Deployment/HardwareAsAService.md) 製品（Nodl、RaspiBlitz、Umbrel など）も含まれます。
:::

### 4.1 コミュニティに質問する（一般的な問題）

基本的な問題に素早く回答を得るには、[BTCPay Mattermost](https://chat.btcpayserver.org/btcpayserver/channels/support) の #support チャンネルで質問するのが最適です。

### 4.2 GitHub で Issue を作成する（高度な問題）

カスタムビルド環境で複雑な問題に直面している場合は、開発者が対応できるよう [GitHub で issue を作成](https://github.com/btcpayserver/btcpayserver/issues)してください。

### 4.3 Premium Support

一部のコミュニティメンバーは有償サポートを提供しています。
より迅速なサポートが必要な場合は、[premium support 提供メンバーの一覧](./Support.md)を確認してください。

### 4.4 Lightning Network サポート

Lightning Network 実装に関する技術的問題がある場合は、それぞれのコミュニティで質問することを検討してください。

#### 4.4.1 LND サポート

- [LND GitHub](https://github.com/lightningnetwork/lnd/issues)
- [Lightning Community on Slack](https://lightningcommunity.slack.com)

#### 4.4.2 Core Lightning (CLN) サポート

- [CLN GitHub](https://github.com/ElementsProject/lightning/issues)
- [CLN Telegram Group](https://t.me/lightningd)
- [CLN docs](https://lightning.readthedocs.io/)
