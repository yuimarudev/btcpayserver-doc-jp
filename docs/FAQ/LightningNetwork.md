<!-- legacy-anchor-aliases -->
<span id="as-a-merchant-do-i-need-to-open-direct-channels"></span>
<span id="can-i-use-a-pruned-node-with-ln-in-btcpay"></span>
<span id="can-i-use-my-existing-ln-node-with-btcpay"></span>
<span id="how-can-i-get-inbound-capacity-to-my-node"></span>
<span id="how-many-users-can-use-lightning-network-in-btcpay"></span>
<span id="how-to-announce-an-ipv6-address"></span>
<span id="how-to-change-from-core-lightning-cln-to-lnd-or-vice-versa"></span>
<span id="how-to-change-my-lnd-node-alias"></span>
<span id="how-to-change-my-LND-Node-alias"></span>
<span id="how-to-disable-on-chain-payments-and-use-ln-payments-only"></span>
<span id="how-to-display-my-lightning-node-information-so-that-others-can-connect-to-me"></span>
<span id="how-to-edit-lightningconfig"></span>
<span id="how-to-edit-lndconf"></span>
<span id="how-to-find-node-info-and-open-a-direct-channel-with-a-store-using-btcpay"></span>
<span id="how-to-install-thunderhub"></span>
<span id="how-to-restart-my-core-lightning-cln"></span>
<span id="how-to-restart-my-lnd"></span>
<span id="how-to-see-lnd-logs"></span>
<span id="how-to-see-my-lightning-network-version"></span>
<span id="i-get-warning-the-lightning-alias-variable-is-not-set-defaulting-to-a-blank-string-when-starting-container"></span>
<span id="i-previously-installed-btcpayserver-without-lightning-can-i-enable-it"></span>
<span id="i-switched-lightning-network-implementation-but-getting-no-payment-available-error"></span>
<span id="lightning-network-core-lightning-cln-faq"></span>
<span id="lightning-network-general-faq"></span>
<span id="lightning-network-lnd-faq"></span>
<span id="lightning-network-questions-and-support"></span>
<span id="lnd-connection-issues-after-an-update"></span>
<span id="what-s-the-default-directory-of-lnd-in-btcpay"></span>
<span id="where-can-i-find-recovery-seed-backup-for-my-lightning-network-wallet-in-btcpay-server"></span>
<span id="which-macaroon-needs-to-be-provided-for-external-nodes"></span>
<!-- /legacy-anchor-aliases -->

# Lightning Network よくある質問

このドキュメントでは、BTCPay の Lightning Network に関して、ユーザーがよく直面する質問や問題を整理しています。オフチェーンプロトコルを使い始める前に、リスクを把握してください。あわせて [BTCPay での Lightning Network 入門](../LightningNetwork.md) もお読みください。

[[toc]]

## Lightning Network 一般 FAQ

ここでは、実装の種類に関係なく、BTCPay の LN に関する一般的な質問を扱います。

### BTCPay では何人が Lightning Network を利用できますか？

セルフホストサーバーでは、内部 Lightning ノードは 1 つだけ利用できます。サーバー所有者は、自分の管理者アカウントに紐づく無制限のストアで同じ Lightning ノードを利用できます。

バージョン 1.0.3.128 以降、BTCPay Server ホストは登録ユーザーに内部 Lightning Network ノードの利用を許可できます。  
これは `Server Settings > Policies > Allow non-admins to use the internal lightning node in their stores` で有効化できます。

![他ユーザー向け LN の有効化](../img/ThirdPartyEnableLNOthers.png)

:::warning サードパーティホストとして
登録ユーザーの資金はすべてあなた自身の Lightning ウォレットに入ります。  
その後、各所有者へ手動で確認・再送金する必要があります。負担になる可能性があります。
:::

:::danger サードパーティホストを利用する個人として
Lightning Network 経由の支払いはすべてサードパーティのウォレットに入ります。  
資金を確実に取り戻せるよう、信頼できるサードパーティホストを使う場合にのみこの方法を使い、十分注意してください。
:::

管理者以外のユーザーは、自分の外部ノードへ接続することもできます。Lightning ノードの外部接続は高度な技術作業です。Lightning を使いたい場合は、必要な構成が一式そろっている自前サーバーのデプロイを推奨します。

### BTCPay を使うストアのノード情報を確認し、直接チャネルを開くには？

Lightning Network の請求書を支払いたい顧客の場合:

1. コイン選択で「Lightning」を選択します。
2. `Copy/Scan` を選択します。
3. `Node Info` を選択し、スキャンまたは手動コピーします。

![BTCPay チェックアウト](../img/btcpay-node-info.jpg)

直接 Lightning Network チャネルを開く具体的な手順は、使用しているウォレットによって異なります。ただし、加盟店のノード情報があれば、進め方はすぐに分かるはずです。

### 加盟店として、直接チャネルを開く必要はありますか？

加盟店には受信チャネルが必要です。他者が加盟店にチャネルを開くと、加盟店側の流動性が増えます。顧客はあなたに直接チャネルを開けるはずです。

接続性の高いノードに、あなた宛ての直接チャネル開設を依頼することもできます。チャネル開設は資金の消費ではなく、プリペイドカードに資金を入れて後で使う、またはチャネルを閉じて引き出すのに近いイメージです。

### 自分のノードで受信容量（inbound capacity）を得るには？

受信容量（inbound capacity）を得る方法は多数あります。実践的なヒントとして、この [inbound capacity に関する記事](https://medium.com/lightningto-me/practical-solutions-to-inbound-capacity-problem-in-lightning-network-60224aa13393) を推奨します。  
受信容量（inbound capacity）を依頼する際は、サービス側のルーティングポリシー手数料も考慮してください。

### 以前 Lightning なしで BTCPayServer をインストールしました。後から有効化できますか？

BTCPay Server では、最初のストア作成後でもいつでも Lightning を設定できます。  
現在、以下 3 つの Lightning Network 実装をサポートしています。

- [LND](https://github.com/lightningnetwork/lnd)
- [Core Lightning (CLN)](https://github.com/ElementsProject/lightning)
- [eclair](https://github.com/ACINQ/eclair)

![ストアへの Lightning 接続](../img/FAQ/btcpaylightningfaq1.jpg)

ストアでの Lightning 設定をさらに学びたい場合は、[Lightning Network](../LightningNetwork.md) ガイドをご覧ください。ストアを Lightning Network に接続できます。

### BTCPay で LN と一緒に pruned node を使えますか？

この実装は pruned node をサポートしているため、Core Lightning (CLN) の利用を推奨します。

### 既存の LN ノードを BTCPay と一緒に使えますか？

すでに十分な受信流動性（inbound liquidity）を持つ接続性の高い Lightning ノードがある場合、同梱ノードではなく既存ノードを BTCPay で使いたいかもしれません。

その場合は、ストアの Lightning ノード設定ページ（`Store > Settings > Lightning > Modify`）に移動します。そこで `Use a custom lightning node` を選択します。

接続文字列は Lightning 実装ごとに異なります。詳細は [設定ページ](../LightningNetwork.md#connecting-an-external-lightning-node-in-btcpay) の接続設定ドキュメントを参照してください。

### Core Lightning (CLN) から LND、またはその逆に変更するには？

:::warning
切り替え前に、チャネルをすべて閉じ、Lightning ノードからオンチェーン資金・Lightning 資金の両方を引き出しておいてください。

:::

仮想マシンへ SSH ログインする必要があります。

LND に切り替える場合:

```bash
sudo su -
cd btcpayserver-docker
export BTCPAYGEN_LIGHTNING="lnd"
. ./btcpay-setup.sh -i
```

Core Lightning (CLN) に切り替える場合:

```bash
sudo su -
cd btcpayserver-docker
export BTCPAYGEN_LIGHTNING="clightning"
. ./btcpay-setup.sh -i
```

### Lightning Network 実装を切り替えた後、`no payment available` エラーが出ます

実装を切り替えた場合、ストア単位で Lightning 接続文字列を適切な実装に合わせて再設定する必要があります。`Stores > Settings > Lightning > Setup > Connection string` に進み、接続文字列画面の `click here` リンクをクリックしてください。

### コンテナ起動時に `WARNING: The LIGHTNING_ALIAS variable is not set. Defaulting to a blank string` が表示されます

これは無視して問題ありません。  
Lightning ノードのエイリアスを設定したい場合は、環境変数ファイルを開きます。

```bash
sudo su -
vim $BTCPAY_ENV_FILE
```

そして `LIGHTNING_ALIAS` エントリを `LIGHTNING_ALIAS=myawesomenode` に追加または変更してください。

### 他のユーザーが接続できるように、自分の Lightning ノード情報を表示するには？

他のユーザーがあなたのノードへ接続するために必要な情報は、すでにチェックアウト画面に表示されています。加盟店によっては、顧客が事前接続できるようノード情報を表示したい場合があります。

ノード情報を確認する方法はいくつかありますが、他者へ表示する最も簡単な方法は Lightning ノード情報ページの利用です。`Store > Settings > Lightning > Modify` に移動してください。ページ下部に `Open Public Node Page` ボタンがあります。クリックすると情報が表示されます。このページは `<iframe>` で Web サイトへ埋め込めます。

![BTCPay チェックアウト](../img/LightningNodepPageInfo.png)

### BTCPay Server の Lightning Network ウォレットのリカバリーシードバックアップはどこで確認できますか？

以前の BTCPay は `noseedbackup` を使用していたため、LN ウォレットのバックアップやリカバリーシードの取得はできませんでした。これは当時、Lightning Network ではチャネル内資金のバックアップ手段がなく、オンチェーンウォレットのみが対象だったためです。  
現在の LND には、シードの存在に依存する static channel backup などの機能があります。  
ただし、Lightning Network は依然として実験段階であることを理解し、[失ってもよい範囲](https://www.youtube.com/watch?v=5fMv8MpzLgQ) の資金のみを扱ってください。

#### シードありで LND を使う（[`v1.0.3.138`](https://github.com/btcpayserver/btcpayserver/releases/tag/v1.0.3.138) 以降）

LND Seed Backup サービスは以下にあります:

- `Server Settings > Services > LND Seed Backup`

![LND Seed Backup サービス](../img/LND-Service-Seed-Backup.jpg)

リカバリーシードは安全にバックアップして保管してください。シードはオンチェーン Lightning ウォレットのバックアップであり、static channel backup 実行にも必要です。

![LND Seed Backup の例](../img/LND-With-Seed-Example.jpg)

安全にバックアップできたら、サーバー上から削除できます。

古いバージョンから `v1.0.3.138` へ移行する場合、移行方法については [このブログ記事](https://blog.btcpayserver.org/btcpay-lnd-migration) が役立つかもしれません。

### オンチェーン決済を無効にして LN 決済だけを使うには？

方法は簡単に 2 つあります:

1. `Store > Settings > Checkout experience > Choose default payment method at checkout`
2. `Store > Settings > Modify > Uncheck the Enabled box` を実行してオンチェーン決済を無効化

### Lightning Network のバージョン確認方法は？

Lightning Network のバージョンはコマンドラインから確認できます。  
LND の場合:

```bash
sudo su -
cd btcpayserver-docker
./bitcoin-lncli.sh help
```

Core Lightning (CLN) の場合:

```bash
sudo su -
./bitcoin-lightning-cli.sh getinfo
```

Lightning ノードへリモート接続できる多くのウォレット（RTL、Zap、Zeus など）でも、フロントエンド上にバージョンが表示されます。

### Lightning Address をリダイレクトする方法は？

ユースケース: `pay.example.com` で BTCPay サーバーを運用しているが、`me@pay.example.com` より見た目が良い `me@example.com` を Lightning Address として使いたい場合。

必要なのは、`example.com/.well-known/lnurlp/me` から `pay.example.com/.well-known/lnurlp/me` への `301` リダイレクト設定だけです。  
Web サーバー設定で実施できます。以下は nginx の例です:

```nginx
server {
  server_name example.com;

  # Redirect Lightning Address requests to BTCPay Server
  rewrite ^/\.well-known/lnurlp/(.*)$ https://pay.example.com/.well-known/lnurlp/$1 permanent;
}
```

## Lightning Network LND よくある質問

ここでは、Lightning Network の [LND 実装](https://github.com/lightningnetwork/lnd/issues) に関する一般的な質問を扱います。

### LND を再起動するには？

```bash
sudo su -
docker restart btcpayserver_lnd_bitcoin
```

### LND のオンチェーンウォレットを再スキャンするには？

:::warning
このフラグメントは、lnd のオンチェーンウォレット取引をリセットして再スキャンを発火させる目的で、一時的にのみ有効化してください。  
再スキャン成功後は必ずこのフラグメントを無効化してください。無効化しないと、再起動のたびにオンチェーンウォレットを再スキャンします。  
WARNING: 再スキャンで取得できるのは、アーカイブ済みブロック内の取引のみです（PRUNED ノードに注意）。
:::
環境変数では指定できない LND 設定を調整するには、`docker-compose-generator/docker-fragments/opt-lnd-wallet-rescan.custom.yml` に [カスタムフラグメント](../Docker/README.md#how-can-i-customize-the-generated-docker-compose-file) を次のように作成します:

```
version: "3"
services:
  lnd_bitcoin:
    environment:
      LND_EXTRA_ARGS: |
        reset-wallet-transactions=1
  lnd_litecoin:
    environment:
      LND_EXTRA_ARGS: |
        reset-wallet-transactions=1
  lnd_bitcoingold:
    environment:
      LND_EXTRA_ARGS: |
        reset-wallet-transactions=1
```

この LND 機能の詳細は、[公式ドキュメント](https://github.com/lightningnetwork/lnd/blob/master/docs/recovery.md#forced-in-place-rescan) を参照してください。

### LND ログを確認するには？

BTCPay Server（Docker インストール）で LND ノードのログを確認するには、次のコマンドを使います:

`docker logs --tail 40 btcpayserver_lnd_bitcoin`

`40` は任意の数に変更できます。これは表示するログ行数を表します。ログの詳細は [Troubleshooting ページ](../Troubleshooting.md) を参照してください。

### BTCPay における LND のデフォルトディレクトリは？

`/var/lib/docker/volumes/generated_lnd_bitcoin_datadir/_data`

### 外部ノード接続時にはどの macaroon を提供すべきですか？

BTCPay Server は、請求書作成前に Lightning ノードが完全同期済みか確認するため `admin.macaroon` を必要とします。  
BTCPay Server 接続専用に macaroon を調整したい場合は、LND macaroon bakery を使ってください:

```bash
lncli bakemacaroon address:read address:write info:read invoices:read invoices:write onchain:read
```

### アップデート後の LND 接続問題

アップデート後、LND では認証失敗が起きることがあります。症状は次のとおりです:

- ストア設定でノード接続テスト時に `Error while connecting to the API (The HTTP status code of the response was not expected (500).)` が出る
- Zap ウォレットで `Unable to connect to host: cannot retrieve macaroon: cannot get macaroon: root key with id 0 doesn’t exist` が出る

この場合、lnd の macaroon を削除して再起動する必要があります。

Docker デプロイを使っている場合は、VM に SSH 接続して次のコマンドを実行してください:

```bash
sudo su -
docker exec btcpayserver_lnd_bitcoin rm /data/admin.macaroon
docker exec btcpayserver_lnd_bitcoin rm /data/invoice.macaroon
docker exec btcpayserver_lnd_bitcoin rm /data/readonly.macaroon
docker exec btcpayserver_lnd_bitcoin rm /data/data/macaroons.db
docker exec btcpayserver_lnd_bitcoin rm /data/data/chain/bitcoin/mainnet/invoice.macaroon
docker exec btcpayserver_lnd_bitcoin rm /data/data/chain/bitcoin/mainnet/macaroons.db
docker exec btcpayserver_lnd_bitcoin rm /data/data/chain/bitcoin/mainnet/readonly.macaroon
docker restart btcpayserver_lnd_bitcoin
```

macaroon が存在しない場合はエラーメッセージが出ますが、安全に無視できます。

これにより以前の macaroon は無効になるため、Zap とは `Server Settings / Services / LND-gRPC` で手動再接続が必要です。

### LND ノードのエイリアスを変更するには？

LND ノードの表示名を変更するには、仮想マシンへ ssh ログインして以下を実行します:

```bash
sudo su -
cd btcpayserver-docker
export LIGHTNING_ALIAS="namehere"
. ./btcpay-setup.sh -i
```

### lnd.conf を編集するには？

環境変数で指定できない [LND 設定](https://docs.lightning.engineering/lightning-network-tools/lnd/lnd.conf) を調整するには、`docker-compose-generator/docker-fragments/opt-lnd-config.custom.yml` に [カスタムフラグメント](../Docker/README.md#how-can-i-customize-the-generated-docker-compose-file) を次のように作成します:

```yml
version: '3'
services:
  lnd_bitcoin:
    environment:
      LND_EXTRA_ARGS: |
        minchansize=1234567
```

`LND_EXTRA_ARGS` の値にカスタマイズを追加できます。例では `minchansize` を設定しています。

その後、追加フラグメントへ設定を加えてセットアップを実行します:

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-lnd-config.custom"
. ./btcpay-setup.sh -i
```

この方法でカスタム設定は config に追加され、アップデート後も維持されます。

### LND watchtower に接続するには？

LND watchtower に接続するには、[`opt-lnd-wtclient`](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/docker-fragments/opt-lnd-wtclient.yml) フラグメントを組み込み、必要に応じて `LND_WTCLIENT_SWEEP_FEE` を設定します:

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-lnd-wtclient"
export LND_WTCLIENT_SWEEP_FEE=10 # Fee to be used for sweep transaction, 10 sat/vbyte is the default
. ./btcpay-setup.sh -i
```

その後、watchtower 接続管理には `wtclient` RPC コマンドを使えます:

```bash
# Connect to a remote watchtower
./bitcoin-lncli.sh wtclient add PUBKEY@IP:PORT

# See your watchtower connections
./bitcoin-lncli.sh wtclient towers
```

### LND watchtower を実行するには？

LND インスタンスと並行して watchtower を実行するには、[`opt-lnd-watchtower`](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/docker-fragments/opt-lnd-watchtower.yml) フラグメントを組み込みます:

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-lnd-watchtower"
. ./btcpay-setup.sh -i
```

これでサーバー上で watchtower が利用可能になります。

他の watchtower クライアント（[wtclient RPC commands]() 経由）からの接続を許可するには、`docker-compose-generator/docker-fragments/opt-lnd-config.custom.yml` の [カスタムフラグメント](../Docker/README.md#how-can-i-customize-the-generated-docker-compose-file) に `watchtower.externalip` を次のように追加する必要があります:

```yml
version: '3'
services:
  lnd_bitcoin:
    environment:
      LND_EXTRA_ARGS: |
        watchtower.externalip=YOUR_SERVER_IP
```

その後、追加フラグメントへ設定を加え、ファイアウォールでポート `9911` を開放します:

```bash
# Add the custom LND fragment
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-lnd-config.custom"
. ./btcpay-setup.sh -i

# Open port the watchtower RPC port in the firewall
ufw allow 9911/tcp
```

`tower info` コマンドを実行すると、`uris` セクションに公開 watchtower インスタンスが表示されるはずです。

```bash
# ./bitcoin-lncli.sh tower info
{
    "pubkey": "YOUR_TOWER_PUBKEY",
    "listeners": [
        "172.23.0.9:9911",
        "127.0.0.1:9911"
    ],
    "uris": [
        "YOUR_TOWER_PUBKEY@YOUR_SERVER_IP:9911"
    ]
}
```

[watchtower 設定](https://docs.lightning.engineering/lightning-network-tools/lnd/watchtower) も参照してください。

### ThunderHub をインストールするには？

インスタンスに ThunderHub をインストールするには、以下を実行します:

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-add-thunderhub"
. btcpay-setup.sh -i
```

`Unable to connect to this node` という警告が出る場合、LND 通信で使う証明書に正しいドメインが含まれていない可能性があります。LND は既存証明書を削除しない限り新しい証明書を生成しません。

古い証明書と鍵を削除して LND に新規生成させるには、以下を実行します:

```bash
docker exec btcpayserver_lnd_bitcoin rm /data/tls.cert
docker exec btcpayserver_lnd_bitcoin rm /data/tls.key
docker restart btcpayserver_lnd_bitcoin
docker restart generated_bitcoin_thub_1
```

## Lightning Network Core Lightning (CLN) よくある質問

ここでは、Lightning Network の [Core Lightning (CLN)](https://github.com/ElementsProject/lightning/issues) 実装に関する一般的な質問を扱います。

### Core Lightning (CLN) を再起動するには？

```bash
sudo su -
docker restart btcpayserver_clightning_bitcoin
```

### IPv6 アドレスをアナウンスするには？

まず `bitcoin-clightning.yml` を Docker フラグメントフォルダへ `bitcoin-clightning.custom.yml` としてコピーします。  
重要: ファイル名は必ず `.custom.yml` で終える必要があります。そうしないと `btcpay-update.sh` 実行時に git コンフリクトが発生します。

新しい `bitcoin-clightning.custom.yml` を次のように変更します:

```yaml
services:
  clightning_bitcoin:
    environment:
      LIGHTNINGD_OPT: |
        announce-addr=[ipv6 here]
```

アドレスは 2 つの角括弧 `[]` の間に入れてください。

続いてセットアップします:

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="bitcoin-clightning.custom"
. ./btcpay-setup.sh -i
```

### .lightning/config を編集するには？

環境変数で指定できない [Core Lightning 設定](https://docs.corelightning.org/reference/lightningd-config) を調整するには、`docker-compose-generator/docker-fragments/opt-lightningd-config.custom.yml` に [カスタムフラグメント](../Docker/README.md#how-can-i-customize-the-generated-docker-compose-file) を次のように作成します:

```yml
version: '3'
services:
  clightning_bitcoin:
    environment:
      LIGHTNINGD_OPT: |
        alias=MyNodeName
        rgb=003366
```

`LIGHTNINGD_OPT` の値にカスタマイズを追加できます。例では `alias` と `rgb` を設定しています。

その後、追加フラグメントへ設定を加えてセットアップを実行します:

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-lightningd-config.custom"
. ./btcpay-setup.sh -i
```

この方法でカスタム設定は config に追加され、アップデート後も維持されます。

## Lightning Network の質問とサポート

プロトコルが比較的新しいため、Lightning Network に関するコミュニティサポートはまだ限定的です。

ここに記載されていない技術的な問題が Lightning Network 実装で発生した場合は、それぞれのコミュニティで質問することを検討してください。

#### LND サポート

- [LND GitHub](https://github.com/lightningnetwork/lnd/issues)
- [Slack の Lightning コミュニティ](https://lightningcommunity.slack.com)

#### Core Lightning (CLN) サポート

- [CLN GitHub](https://github.com/ElementsProject/lightning/issues)
- [CLN Telegram グループ](https://t.me/lightningd)
- [CLN ドキュメント](https://lightning.readthedocs.io/)
