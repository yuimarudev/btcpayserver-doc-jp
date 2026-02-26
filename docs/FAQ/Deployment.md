<!-- legacy-anchor-aliases -->
<span id="after-initial-deployment-i-can-t-register-and-i-don-t-have-a-login-yet"></span>
<span id="are-there-free-hosts-where-i-can-test"></span>
<span id="can-i-connect-to-my-btcpay-bitcoin-p2p-on-port-8333"></span>
<span id="can-i-deploy-btcpay-on-my-existing-vps"></span>
<span id="can-i-run-btcpay-on-my-own-hardware"></span>
<span id="can-i-run-btcpay-on-my-home-computer"></span>
<span id="can-i-start-btcpay-only-when-i-m-expecting-a-payment"></span>
<span id="can-i-use-an-existing-nginx-server-as-a-reverse-proxy-with-ssl-termination"></span>
<span id="can-i-use-bitcoin-knots-instead-of-bitcoin-core"></span>
<span id="cause-3-btcpay-is-expecting-you-to-access-this-website-from"></span>
<span id="cause-4-getting-500-nginx-error-on-a-local-server-https-and-for-http-btcpay-is-expecting-you-to-access-this-website-from"></span>
<span id="general-deployment-faq"></span>
<span id="how-can-i-modify-or-deactivate-environment-variables"></span>
<span id="how-can-i-renew-my-ssl-certificate"></span>
<span id="how-can-i-run-btcpay-on-testnet"></span>
<span id="how-do-i-activate-tor-on-my-btcpay-server"></span>
<span id="how-do-i-completely-uninstall-btcpay-from-a-linux-environment-docker-version"></span>
<span id="how-do-i-disable-tor-on-my-btcpay-server"></span>
<span id="how-much-does-it-cost-to-run-btcpay-server"></span>
<span id="how-to-access-the-onion-address-without-clearnet"></span>
<span id="how-to-change-domain-name-on-my-lunanode-btcpay"></span>
<span id="how-to-change-your-btcpay-server-domain-name"></span>
<span id="how-to-choose-a-proper-deployment-method"></span>
<span id="how-to-deploy-btcpay-server-alongside-existing-bitcoin-node"></span>
<span id="how-to-manually-install-btcpay-on-ubuntu-18-04"></span>
<span id="luna-node-web-deployment-faq"></span>
<span id="manual-deployment"></span>
<span id="setting-up-dns-records"></span>
<span id="web-deployment-faq"></span>
<span id="what-are-the-minimal-requirements-for-btcpay"></span>
<span id="what-is-the-easiest-method-to-deploy-a-self-hosted-btcpay-server"></span>
<span id="why-activate-tor-does-it-mean-that-nobody-knows-who-i-am"></span>
<span id="with-the-docker-deployment-how-to-use-a-different-volume-for-the-data"></span>
<!-- /legacy-anchor-aliases -->

# デプロイ FAQ

このドキュメントでは、ソフトウェアのインストール前およびインストール中に遭遇する、最も一般的な質問・エラー・問題を扱います。デプロイ方法の詳細な一覧と各手順については、[デプロイページ](../Deployment/README.md) を参照してください。

[[toc]]

## 一般的なデプロイ

### BTCPay Server の運用にはどれくらい費用がかかりますか？

BTCPay は 100% 無料のオープンソースソフトウェアです。私たちが料金を請求することはありません。
ただし、運用するにはホスティングが必要です。自分のローカルサーバーでセルフホストすることも、クラウドホスティングプロバイダーを利用することもできます（多くのユーザーはこちらを選択しています）。上級ユーザーは [自前ハードウェア](/Deployment/Hardware.md) 上で BTCPay を実行できます。技術的な知識が少ないユーザーは [Hardware as a Service オプション](/Deployment/HardwareAsAService.md) を利用できます。自分でサーバーをホストしたくない場合は、無料の [サードパーティホスト](/Deployment/ThirdPartyHosting.md) も利用できます。BTCPay の実行方法については、[デプロイページ](/Deployment/README.md) で詳しく確認してください。

### BTCPay の最小要件は何ですか？

Bitcoin と Lightning Network ノードを実行する場合の最小要件は次のとおりです。

- 2GB RAM
- 80 GB のストレージ（[pruning を有効化](../Docker/README.md#generated-docker-compose)）
- Docker

### セルフホストの BTCPay Server をデプロイする最も簡単な方法は何ですか？

初心者には、セルフホストを希望する場合は [Web デプロイ](/Deployment/LunaNode.md)、または [サードパーティホスト](/Deployment/ThirdPartyHosting.md) の利用を強く推奨します。

複数の暗号通貨コインを追加する場合は、それぞれのブロックチェーンサイズに応じてストレージを増やす必要があります。

### 適切なデプロイ方法はどう選べばよいですか？

インストール方法の比較については [デプロイページ](/Deployment/README.md) を参照し、自分のニーズとスキルレベルに最も合う方法を選択してください。

### BTCPay を自分のハードウェアで動かせますか？

はい、可能です。詳細な手順は [ハードウェアデプロイページ](/Deployment/Hardware.md) を確認してください。

### 既存の VPS に BTCPay をデプロイできますか？

はい。BTCPay はドキュメント化されたデプロイ方法に限定されません。最小要件を満たす限り、好みのホスティングソリューションを利用できます。

### 試せる無料ホストはありますか？

セルフホストの BTCPay では、無制限のユーザーとストアを紐付けられます。一部のコミュニティユーザーは、主にテストや学習目的で他の人が使えるよう、サーバーを公開登録にしています。その多くはコミュニティ主導で無料です。詳細は [サードパーティホストのドキュメント](/Deployment/ThirdPartyHosting.md) を参照してください。

### 初回デプロイ後、まだログインがなく登録もできません

BTCPay Server をデプロイしたら、最初にユーザー登録を行う必要があります（サーバー同期中）。この最初のユーザーは自動的にサーバー管理者になります。初回デプロイ後にヘッダーメニューが Login しか表示されず、最初のユーザー登録ができない場合は、すでに別の誰かが管理者として登録した可能性があります。発生確率は高くありませんが（そのユーザーがあなたの BTCPay ドメイン名を知り監視している必要があります）、SSH の秘密鍵にアクセスされた可能性があるため、セキュリティ上の理由で新しいサーバーを再デプロイしてください。

### BTCPay Server で Tor を有効化するには？

Docker デプロイでは Tor はデフォルトで有効です。

### BTCPay Server で Tor を無効化するには？

非常に簡単です。SSH でインスタンスにログインし、root で次のコマンドを実行してください。

```bash
BTCPAYGEN_EXCLUDE_FRAGMENTS="$BTCPAYGEN_EXCLUDE_FRAGMENTS;opt-add-tor"
. btcpay-setup.sh -i
```

その後、サーバーが再起動するまで数分待てば完了です。

### なぜ Tor を有効化するのですか？ 有効化すると誰にも身元が分からなくなりますか？

BTCPay Server における Tor は、主にセットアッププロセスを改善する目的で提供されており、自宅やオフィスの自前デバイスでのホスティングに柔軟性をもたらします。

Tor を有効化すると、次の設定手順が不要になり、よりシンプルなプラグアンドプレイで BTCPay を利用できます。

- ファイアウォールで複数ポートを開放する
- ローカルネットワーク上のデバイスへポート転送するための NAT を設定する
- HTTPS 証明書を取得するために DNS エントリを設定する
- Lightning 用に固定 IP を用意する

これらは通常 VPS で BTCPay をホストする場合は問題になりにくいですが、自宅やオフィスのネットワーク環境では、非技術ユーザーには難しいことがあります。

Tor はこれらの問題を一度に解決します。必要なのはデバイスをローカルネットワークにつなぐだけです。特に POS アプリケーションで有用です。

ただし、完全なプライバシーとセキュリティを求めるなら、**BTCPay で Tor を有効化するだけでは不十分です。**

Tor は開発者にとって非常に扱いが難しいソフトウェアで、わずかなミスでも匿名性が損なわれる可能性があります。BTCPay はより複雑なサービスへ進化し、プラグインも増え続けているため、すべての通信を Tor 経由にしようとしても、平文データの漏えいが絶対に起こらないとは保証できません。

私たちは、セキュリティがない状態よりも、あるいは不完全だと分かっているセキュリティよりも、「安全だと思い込むこと」のほうが危険だと考えています。そのため、Tor を有効化しても、他者があなたのインスタンス Web サイトや Bitcoin / Lightning ノードへ平文で接続すること自体は防げず、**匿名にはまったくなりません。**

この背景にある考え方について詳しく知りたい場合は、[Medium の記事](https://medium.com/@BtcpayServer/about-tor-and-btcpay-server-2ec1e4bd5e51) を参照してください。

### clearnet を使わずに .onion アドレスへアクセスするには？

clearnet 経由でアクセスして左上の Tor ロゴをクリックしなくても、次のコマンドで BTCPay インスタンスの .onion アドレスを確認できます。

```bash
cat /var/lib/docker/volumes/generated_tor_servicesdir/_data/BTCPayServer/hostname
```

### 環境変数を変更または無効化するには？

BTCPay では、さまざまなオプションが環境変数で有効化されています。これらのオプションは、`export {environment variable}="{value}"` で新しい値をエクスポートし、その後 `. ./btcpay-setup.sh -i` を再実行することで、コマンドラインから変更または削除できます。

例えば、BTCPay サーバーの Tor を無効化したい場合は次のとおりです。

```bash
# Login as root
sudo su -

# Go to the root/btcpayserver-docker directory
cd /root/btcpayserver-docker

# Print the complete list of options that you are running (for the sake of the demonstration, let's say that besides Tor you have pruning mode activated too)
echo $BTCPAYGEN_ADDITIONAL_FRAGMENTS
opt-save-storage-s;opt-add-tor

# Export the BTCPAYGEN_ADDITIONAL_FRAGMENTS variable without opt-add-tor
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-save-storage-s"

# Run btcpay-setup.sh
. btcpay-setup.sh -i

exit
```

同様に、環境変数を追加する場合は、`export` コマンドは次のようになります。

```bash
# Enable Tor in addition to your existing environment variables (such as pruning)
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-add-tor"
```

どの環境変数を変更すべきか確認したい場合は、[この一覧](https://github.com/btcpayserver/btcpayserver-docker#environment-variables) を参照してください。

### BTCPay を testnet で実行するには？

上のセクションを踏まえると、デフォルトの `mainnet` ではなく `testnet` を使う設定は次のとおりです。

```bash
# Export the NBITCOIN_NETWORK variable switching to testnet
export NBITCOIN_NETWORK="testnet"

# Run btcpay-setup.sh for the change to take effect
. btcpay-setup.sh -i
```

すべてを自分でデプロイせずに素早く試したい場合は、[試してみる](../TryItOut.md) セクションを確認してください。
ここには、私たちがホストしている BTCPay の testnet インスタンスへのリンクと説明があります。

### 支払いを受ける予定があるときだけ BTCPay を起動できますか？

いいえ。Bitcoin ノードがブロックチェーンと同期してトランザクションを検証できるよう、BTCPay は常時稼働させる必要があります。ときどき起動するだけだと最新ブロックの検証追従に時間がかかり、支払い反映が大幅に遅れます。

### ポート 8333 で BTCPay の Bitcoin P2P に接続できますか？

BTCPay の Bitcoin Core ノードは、デフォルトでは外部公開されていません。BTCPay の用途では帯域要件を増やすため、通常これはユーザーの利益になりません。また BTCPay はこのポートへの接続をホワイトバインドしているため、公開すると DDoS の潜在的リスクにさらされます。

ただし、Tor 経由ではフルノードへの P2P 接続を公開しています。Tor アドレスは次のコマンドで取得できます。

```bash
cat /var/lib/docker/volumes/generated_tor_servicesdir/_data/BTC-P2P/hostname
```

または、管理者でログインした BTCPay Server インスタンスの `Server Settings` から確認できます。

この Tor hidden service は信頼できない相手と共有しないでください。この hidden service への接続は Bitcoin ノード側でホワイトリストされるため、悪意あるピアがノードを DDoS できる可能性があります。

bitcoind の P2P ポート 8333 を安全性を下げて公開する必要がある場合（例: Bisq、DOJO、Esplora などで P2P が必要な場合）で、Docker デプロイを使用しているなら、追加フラグメント [opt-unsafe-expose](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を利用できます。

:::danger WARNING
信頼できる LAN 上、または特定ホストのみを許可するファイアウォールのホワイトリスト設定と併用する場合にのみ使用してください
:::

### SSL 証明書を更新するには？

BTCPay Server の SSL 証明書が期限切れになった場合、手動で更新できます。Docker デプロイでは、サーバー上の `letsencrypt-nginx-proxy-companion` という名前の [コンテナを再起動](../Troubleshooting.md#13-restarting-a-service) するのが最も簡単です。

### 既存の Nginx サーバーを SSL 終端付きリバースプロキシとして使えますか？

はい、可能です。適切な設定を使用してください。

`/etc/nginx/sites-available/btcpayserver` に vhost 用の追加設定ファイルを作成し、`/etc/nginx/sites-enabled/btcpayserver` にこのファイルへのシンボリックリンクを作成します。

この vhost ファイルの内容は次のようになります。

```nginx
server {
	listen 80;

	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;

	# Put your domain name here
	server_name btcpay.domain.com;

	# Needed for Let's Encrypt verification
	location ~ /.well-known/acme-challenge {
		allow all;
	}

	# Force HTTP to HTTPS
	location / {
		return 301 https://$http_host$request_uri;
	}
}

server {
	listen 443 ssl http2;

	ssl on;

	# SSL certificate by Let's Encrypt in this Nginx (not using Let's Encyrpt that came with BTCPay Server Docker)
	ssl_certificate      /etc/letsencrypt/live/btcpay.domain.com/fullchain.pem;
	ssl_certificate_key  /etc/letsencrypt/live/btcpay.domain.com/privkey.pem;

	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;

	# Put your domain name here
	server_name btcpay.domain.com;

	# Route everything to the real BTCPay server
	location / {
		# URL of BTCPay Server (i.e. a Docker installation with REVERSEPROXY_HTTP_PORT set to 10080)
		proxy_pass http://127.0.0.1:10080;

		proxy_set_header Host $http_host;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		# For websockets (used by Ledger hardware wallets)
		proxy_set_header Upgrade $http_upgrade;
	}

	# Needed for Let's Encrypt verification
	location ~ /.well-known/acme-challenge {
		allow all;
	}
}

```

また、メインの Nginx 設定ファイル `/etc/nginx/nginx.conf` に次を追加してください。

```nginx
http {

	# ... # Existing stuff

	# Needed to allow very long URLs to prevent issues while signing PSBTs
	server_names_hash_bucket_size 128;
	proxy_buffer_size          128k;
	proxy_buffers              4 256k;
	proxy_busy_buffers_size    256k;
	client_header_buffer_size 500k;
	large_client_header_buffers 4 500k;

	# Needed websocket support (used by Ledger hardware wallets)
	map $http_upgrade $connection_upgrade {
    	default upgrade;
    	''      close;
	}

}
```

次に、`service nginx configtest` で Nginx の設定をテストし、`service nginx reload` で設定を再読み込みします。

続いて、BTCPayServer 側で HTTPS を処理しないようにする必要があります。BTCPayServer インスタンスで無効化してください。

```bash
BTCPAYGEN_EXCLUDE_FRAGMENTS="$BTCPAYGEN_EXCLUDE_FRAGMENTS;nginx-https"
. btcpay-setup.sh -i
```

注意: BTCPay Server のインストールで複数ドメイン（例: `WOOCOMMERCE_HOST` や `BTCPAY_ADDITIONAL_HOSTS`）を使用している場合、各ドメイン名ごとに設定を変更する必要があります。上記の例は `btcpay.domain.com` という 1 つのドメインのみを対象にしています。

### Bitcoin Core の代わりに Bitcoin Knots を使えますか？

可能です。まずデプロイを最近更新済みであること（`btcpay-update.sh`）を確認し、次のコマンドを実行してください。

```bash
export BTCPAYGEN_EXCLUDE_FRAGMENTS="$BTCPAYGEN_EXCLUDE_FRAGMENTS;bitcoin"
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;bitcoinknots"
. btcpay-setup.sh -i
```

## BTCPay Server のドメイン名を変更する方法

### DNS レコードを設定する

ドメインを購入し、それを BTCPay Server に接続したい場合、
通常はホスティング事業者にドメイン管理ページがあります。
そこで `DNS records` ページを開き、`CNAME` レコードを追加します。

このレコードでは、VPS ホストから提供されたドメインを指すように設定します。
IP アドレスで設定することもできますが、その場合は `CNAME record` ではなく `A Record` になります。

これは [gandi.net](https://gandi.net/) での表示例です。

![Gandi3](../img/Gandi3.png)

### BTCPay Server の設定でドメイン名を変更する

BTCPay Server で `Server Settings` メニューを開き、`Maintenance` タブに進みます。
ここに旧ドメインを新ドメインへ置き換える項目があり、更新には数秒かかる場合があります。

![Maintenance domain name](../img/changedomain.png)

次に、アドレスバーへ新しいドメインを入力し、動作するか確認してください。

![Maintenance2](../img/Maintenance2.png)

### コマンドラインでドメインを変更する

SSH 経由でサーバーに接続します。

例:

```bash
ssh btcpayserver@myawesomedemobtcpay.westeurope.cloudapp.azure.com
```

パスワードを入力し、ドメイン名を変更します。

```bash
sudo su -
changedomain.sh tothemoon.btcpayserver.com
```

成功です。

## Web デプロイ

ここでは、BTCPay の Web デプロイに関する一般的な質問と解決策を示します。

### BTCPay を自宅のコンピューターで実行できますか？

Web サイトをホストする要件と同様に、BTCPay Server インスタンスには Web サーバーが必要です。BTCPay Server をローカル PC で動かすこと自体は可能ですが、最小要件を満たし、サービス中断を避けるため 24 時間 365 日稼働させる必要があります。また、BTCPay Server の支払い関連アクティビティに関して自宅 IP アドレスを公開したくない場合も多いでしょう。これらの理由から、ローカルホスティングはテスト用途には適していますが、本番用途には現実的ではありません。一般的には、これらの問題を解決するために Virtual Private Server (VPS) が使用されます。

### LunaNode Web デプロイ

#### LunaNode の BTCPay でドメイン名を変更するには？

1. LunaNode ダッシュボードで、Virtual Machines > Your Virtual Machine > General Tab > External IP を開き、外部 IP をコピーします。
2. DNS プロバイダーで A レコードを作成し、外部 IP を貼り付けます。
3. Server Settings > Maintenance > Change Domain を開き、`http` や `https` のプレフィックスなしで yourdomain.com を貼り付けます。

追加ドキュメント: [BTCPay Server のドメイン名を変更する方法](../FAQ/Deployment.md#how-to-change-your-btcpay-server-domain-name))。

## 手動デプロイ

#### Ubuntu 18.04 に BTCPay を手動インストールするには？

この [コミュニティガイド](https://freedomnode.com/blog/114/how-to-setup-btc-and-lightning-payment-gateway-with-btcpayserver-on-linux-manual-install) を確認してください。

### Linux 環境（Docker 版）から BTCPay を完全にアンインストールするには

[`btcpay-teardown.sh`](https://github.com/btcpayserver/btcpayserver-docker/blob/master/btcpay-teardown.sh) スクリプトを次のように実行します。

```bash
sudo su -
. ./btcpay-teardown.sh
```

これで、インスタンスから BTCPay Server が完全に削除され、関連する Docker コンテナとボリュームも削除されます。

### 既存の Bitcoin ノードと並行して BTCPay Server をデプロイするには？

以下の手順は Docker デプロイ向けです。

- [btcpayserver-docker](https://github.com/btcpayserver/btcpayserver-docker#full-installation-for-technical-users) の説明に従って `. ./btcpay-setup.sh -i` までセットアップを進める
- `docker-compose-generator/docker-fragments/` フォルダに `bitcoin.custom.yml` を作成する

```yml
version: '3'

services:
  btcpayserver:
    environment:
      BTCPAY_CHAINS: 'btc'
      BTCPAY_BTCEXPLORERURL: http://nbxplorer:32838/
  nbxplorer:
    environment:
      NBXPLORER_CHAINS: 'btc'
      NBXPLORER_BTCRPCURL: http://host.docker.internal:43782/
      NBXPLORER_BTCRPCUSER: 'rpc-username'
      NBXPLORER_BTCRPCPASSWORD: 'rpc-password'
      NBXPLORER_BTCNODEENDPOINT: host.docker.internal:39388
    volumes:
      - 'localBitcoinfolder:/root/.bitcoin'
```

- `43782` を、Bitcoin ノードで設定している RPC ポートに置き換える
- `rpc-username` を、Bitcoin ノードで設定している RPC ユーザー名に置き換える
- `rpc-password` を、Bitcoin ノードで設定している RPC パスワードに置き換える
- `39388` を、Bitcoin ノードで設定している p2p ポートに置き換える
- `localBitcoinfolder` を、Bitcoin データフォルダのパスに置き換える

Linux で実行している場合は、[docker の制約](https://github.com/docker/for-linux/issues/264) により次も必要です。

- `ip route | grep docker0 | awk '{print $9}'` を実行する
  - `bitcoin.custom.yml` ファイルの末尾に次を追加し、`$DOCKER_HOST_IP` を前のコマンド結果で置き換える

```yml
extra_hosts:
  - 'host.docker.internal:$DOCKER_HOST_IP'
```

- `export BTCPAYGEN_EXCLUDE_FRAGMENTS="bitcoin"` を実行する
- `export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;bitcoin.custom"` を実行する
- `. ./btcpay-setup.sh -i` を実行する

既存の Lightning ノードと並行デプロイする方法を探している場合は、[こちら](./LightningNetwork.md#can-i-use-my-existing-ln-node-with-btcpay) を参照してください。

### Docker デプロイで、データ用に別ボリュームを使うには？

まず、btcpayserver と docker が動作していないことを確認してください。

```bash
sudo su -
btcpay-down.sh
systemctl stop docker
```

次にドライブをフォーマットします。すでに実施済みならこの手順はスキップできます。

```bash
# Step 1: Unplug the drive
lsblk

# Step 2: Plug the drive
lsblk
```

2 回目の `lsblk` で、今接続したドライブ（TYPE が `disk`）が表示されるはずです。
次のコマンドはこのディスク上のすべてのデータを消去するため、誤りがないよう注意してください。

例として、NAME が `/dev/sdd` だとします。

```bash
# Save the name in a variable
DEVICE_NAME="/dev/sdd"
# Set the partition name
PARTITION_NAME="/dev/sdd1"
```

ここからディスクをパーティション分割し、パーティションをフォーマットします。

```bash
echo "Partitioning the external drive $DEVICE_NAME..."
### DANGER ZONE ###
(
	echo o # Create a new empty DOS partition table
	echo n # Add a new partition
	echo p # Primary partition
	echo 1 # Partition number
	echo   # First sector (Accept default: 1)
	echo   # Last sector (Accept default: varies)
	echo w # Write changes
) | fdisk ${DEVICE_NAME}
partprobe ${DEVICE_NAME}
while ! lsblk $PARTITION_NAME &> /dev/null; do
	sleep 1
done
mkfs.ext4 -F "$PARTITION_NAME"
```

次に、このパーティションを Linux ファイルシステムへマウントします。

```bash
# Mounting the partition
MOUNT_DIR="/mnt/external"
mkdir "$MOUNT_DIR"
mount -o defaults,noatime "$PARTITION_NAME" "$MOUNT_DIR"

# Make sure the partition exists at the next reboot, we use UUID in case
# the partition name is different in the next reboot
if ! grep -qF "$MOUNT_DIR" /etc/fstab; then
	UUID="$(sudo blkid -s UUID -o value $PARTITION_NAME)"
	echo "UUID=$UUID $MOUNT_DIR ext4 defaults,noatime,nofail 0 2" >> /etc/fstab
fi
```

続いて、マウント前に docker が起動しないように設定します。

```bash
MOUNT_UNIT="$(systemd-escape --path "$MOUNT_DIR").mount"
docker_service="/lib/systemd/system/docker.service"
if ! grep -qF "After=$MOUNT_UNIT" "$docker_service"; then
	sed -i "s/After=/After=$MOUNT_UNIT /g" "$docker_service"
fi
```

次に、先ほどのパーティションへ Docker ボリュームデータをすべて置く場合を想定します。

```bash
DOCKER_VOLUMES="/var/lib/docker/volumes"
# Copy all the data from our volume to the mount directory (this can take a while)
cp -a -r "$DOCKER_VOLUMES/." "$MOUNT_DIR"
# Make the folder a mountpoint
rm -rf "$DOCKER_VOLUMES"
mkdir -p "$DOCKER_VOLUMES"
mount --bind "$MOUNT_DIR" "$DOCKER_VOLUMES"
# Make sure the mountpoint is mounted after reboot
if ! grep -qF "$DOCKER_VOLUMES" /etc/fstab; then
	echo "$MOUNT_DIR $DOCKER_VOLUMES none bind,nobootwait 0 2" >> /etc/fstab
fi
```

最後に docker と btcpayserver を再起動します。

```bash
systemctl start docker
btcpay-up.sh
```

注: `docker volume rm` 実行時に docker がエラーを出す可能性があるため、シンボリックリンクではなく mount bind を使用しています。

### 503 Service Temporarily Unavailable nginx が表示される

#### 原因 1: IP アドレスで BTCPay にアクセスしている

nginx は HTTP リクエストを受け取ると、どのサービスが実際の宛先かを判断する必要があります。`BTCPAY_HOST` を `http://raspberrypi.local/` に設定している場合、BTCPay Server にはこの URL でしかアクセスできません。別のドメイン名や IP アドレス（例: `http://192.168.0.2`）でアクセスすると HTTP 503 エラーになります。

```
503 Service Temporarily Unavailable
-----------------------------------
nginx
```

この問題は、該当 HTTP リクエストを BTCPay Server にルーティングするよう nginx に指示することで解決できます。
次のようにセットアップスクリプトを再実行してください。

```bash
sudo su -

REVERSEPROXY_DEFAULT_HOST="$BTCPAY_HOST" && . btcpay-setup.sh -i
```

これで `http://192.168.0.2` へのアクセスも正しく動作するはずです。

#### 原因 2: btcpayserver または letsencrypt-nginx-proxy が起動していない

確認するには次を実行します。

```bash
sudo  docker ps | less -S
```

`less` を終了するには `q` を押します。

出力には次が含まれている必要があります。

- btcpayserver/letsencrypt-nginx-proxy-companion
- btcpayserver/btcpayserver

また、ステータスは `Up` である必要があります。

Docker コンテナが起動していない場合は、次のようにクラッシュ理由を確認してください。

```bash
 sudo  docker logs 6a6b9fd75692 --tail 20
```

ここで `6a6b9fd75692` は問題が起きているコンテナ ID です。

#### 原因 3: BTCPay がこの Web サイトへのアクセス元を想定している

`You access BTCPay Server over an unsecured network` というエラーが表示されることもあります。

このエラーは、BTCPay Server のバージョン `1.0.3.73` 以降でフロントページに表示される場合があります。

これは、同一サーバー上で異なるドメインを扱えるようにするため BTCPay に入った破壊的変更が原因です。

問題が起きるのは、BTCPay Server がインターネットへ直接公開されておらず、代わりにリバースプロキシ（nginx や IIS など）がリクエストを受けて BTCPay Server へ転送している構成です。

残念ながら、リバースプロキシ設定によっては、HTTP HOST ヘッダーが置き換えられているか、または先頭側のプロトコルを示す http ヘッダー `X-Forwarded-Proto` を転送していないことがあります。

NGinx を使っている場合、`/etc/nginx/conf.d/default.conf` のトップレベルに次が必要です。

```nginx
map $http_x_forwarded_proto $proxy_x_forwarded_proto {
  default $http_x_forwarded_proto;
  ''      $scheme;
}
proxy_set_header Host $http_host;
proxy_set_header X-Forwarded-Proto $proxy_x_forwarded_proto;

server_names_hash_bucket_size 128;
proxy_buffer_size          128k;
proxy_buffers              4 256k;
proxy_busy_buffers_size    256k;
client_header_buffer_size 500k;
large_client_header_buffers 4 500k;
```

リバースプロキシが Apache 2 の場合は、次の 2 つの設定が必要です。

```
<VirtualHost *:443>
    RequestHeader set X-Forwarded-Proto "https"
    ProxyPreserveHost on
...
</VirtualHost>
```

PSBT 署名時の問題を防ぐため、`apache2.conf` にも次の設定が必要です。

```
LimitRequestLine 500000
LimitRequestFieldSize 500000
```

#### 原因 4: ローカルサーバーで https は 500 nginx エラー、http は BTCPay is expecting you to access this website from が出る

ポート 80 と 443 を開放する必要があります。開放後、`btcpay-restart.sh` で docker を再起動してください。

#### 原因 5: その他

5XX HTTP エラーの原因は多数あります。[Issue](https://github.com/btcpayserver/btcpayserver-docker/issues) を作成し、原因が判明したらこの [デプロイ FAQ](/FAQ/Deployment.md) ドキュメントへ追記してください。
