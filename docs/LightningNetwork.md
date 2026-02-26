<!-- legacy-anchor-aliases -->
<span id="connecting-an-external-lightning-node-in-btcpay"></span>
<span id="connecting-your-internal-lightning-node-in-btcpay"></span>
<!-- /legacy-anchor-aliases -->

# Lightning Network (LN) と BTCPay Server

## 概要

BTCPay Server のデプロイ後、Bitcoin プロトコル上に構築された革新的なセカンドレイヤー決済システムである [Lightning Network](https://en.bitcoin.it/wiki/Lightning_Network) を試したくなるかもしれません。

このガイドでは、BTCPay Server で Lightning Network (LN) ノードを設定する方法と基本事項を説明します。

現在、BTCPay Server は次の Lightning Network 実装を提供しています。

- [LND](https://github.com/lightningnetwork/lnd)
- [Core Lightning (CLN)](https://github.com/ElementsProject/lightning)（旧 c-lightning）
- [Eclair](https://github.com/ACINQ/eclair)

::: danger
続行する前に、Lightning Network はまだ実験段階であることを理解してください。
Lightning Network の利用には資金損失リスクがあります。失っても問題ない額以上は使わないでください。
:::

Lightning Network 利用に伴うリスクを十分に理解してください。

BTCPay Server で内部 Lightning Node を運用する場合は、次の点を考慮してください。

1. すべての lightning network ノードは **on-chain** と **off-chain** の2層で動作します。
2. 選択した LN 実装は、off-chain の payment channel 資金に使う on-chain hot wallet を作成します。
3. **on-chain** hot wallet のシードを必ずバックアップしてください（実装ごとの手順は後述）。
4. 手順 #3 のシードで復元できるのは **on-chain 資金のみ** です（ただし off-chain 運用には必要です）。
5. チャネルにロックされた **off-chain** 資金は単一シードではバックアップできません。利用する LN 実装のドキュメントを確認してください。
6. **off-chain** の復旧メカニズムは現在も研究・開発中です。BTCPay Server の消去や安全でない運用（例: ファイルシステム破損、鍵の漏えい）は、**恒久的な資金損失**につながる可能性があります。

技術が成熟するにつれ、適切なバックアップ機構は BTCPay Server でより実装しやすくなります。
[v1.0.3.138](https://blog.btcpayserver.org/btcpay-lnd-migration/) 時点では、BTCPay Server で [lightning seed backups](./FAQ/LightningNetwork.md#where-can-i-find-recovery-seed-backup-for-my-lightning-network-wallet-in-btcpay-server) が可能なのは LND のみです。

## Lightning Network 実装の選択

まず、デプロイ前に pruning 済み Bitcoin ノードと lightning network 実装の併用について [こちら](./FAQ/LightningNetwork.md#can-i-use-a-pruned-node-with-ln-in-btcpay) を確認してください。

インストール時に実装を選択できます。

[web-interface installations](/Deployment/LunaNode.md) の場合は、ドロップダウンメニューから実装を選ぶだけです。
それ以外の [docker](https://github.com/btcpayserver/btcpayserver-docker) ベースの [deployment methods](/Deployment/README.md) では次を実行します。

```bash
sudo su -
cd btcpayserver-docker
export BTCPAYGEN_LIGHTNING="implementationgoeshere"
. ./btcpay-setup.sh -i
```

- **Core Lightning (CLN)** は `export BTCPAYGEN_LIGHTNING="clightning"` を使用
- **LND** は `export BTCPAYGEN_LIGHTNING="lnd"` を使用
- **eclair** は `export BTCPAYGEN_LIGHTNING="eclair"` と `export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-txindex"` を使用

最後に、Lightning を使い始めるにはブロックチェーン同期が完了している必要があります。

## BTCPay Server での Lightning ノード設定

### 内部 Lightning Node を接続する

どの LN 実装をデプロイした場合でも、BTCPay Server で内部 Lightning Node を接続する手順は同じです。

1. ストアを選択
2. "Lightning" > "Use internal node" を選択
3. "Save" をクリックし、"BTC Lightning node updated" メッセージを確認
4. "Public Node Info" を開き、ノードが **"Online"** と表示されることを確認

![LightningNetworkNodeSetupOverview](./img/lightning-node-setup/LightningNetworkNodeSetupOverview.jpg)

内部接続に失敗する場合は、次を確認してください。

1. Bitcoin on-chain ノードの同期が完了している
2. "Lightning" > "Settings" > "BTC Lightning Settings" で Internal lightning node が "Enabled" になっている

Lightning ノードに接続できない場合は、[サーバー再起動](./FAQ/ServerSettings.md#how-to-restart-btcpay-server) または [トラブルシューティングガイド](./Troubleshooting.md) を確認してください。Lightning ノードが "Online" になるまで、ストアで lightning 決済は受け付けられません。"Public Node Info" リンクをクリックして接続テストしてください。

### 外部 Lightning Node を BTCPay Server に接続する

BTCPay Server は外部 Lightning ノード接続にも対応しています。設定手順は以下です。

1. Lightning ノード未設定の場合、"Lightning" > "Use custom node" を選択
2. 既存接続を変更する場合、"Lightning" > "Settings" > "Change connection" > "Use custom node" を選択
3. 利用実装に合わせた接続情報を入力し、"Test connection" を実行

## BTCPay Server と LND の開始手順

### Ride The Lightning (RTL) で LND を操作する

BTCPay Server で LND 実装を使う最も簡単な方法は、**[Ride The Lightning](https://github.com/Ride-The-Lightning/RTL)** (RTL) サービスです。RTL は Lightning Network 用の Web UI で、ブラウザから BTCPay Server を離れずにノードを運用できます。
\
BTCPay Server で RTL を開始するには、Server Settings > Services > Ride The Lightning > See information へ進みます。

### Zap で LND を操作する

iOS や PC から LND ノードをリモート利用する場合は、[Zap wallet integration](https://github.com/LN-Zap/zap-tutorials/blob/master/docs/desktop/btcpay-server.mdx) を利用できます。
\
[![LND BTCPay](https://img.youtube.com/vi/CWhTOunTb2Q/mqdefault.jpg)](https://www.youtube.com/watch?v=CWhTOunTb2Q)
\
Zap 以外にも、LND ノードをリモート操作できるウォレットとして [Nayuta wallet](https://nayuta.co/) と [ZeusLN](https://github.com/ZeusLN/zeus) があります。いずれもコミュニティでの十分な検証はまだ行われていません。

### Lightning Joule で LND を操作する

Web ブラウザ経由で LND ノードをリモート操作するには Lightning Joule を利用できます。
\
[![Joule](https://img.youtube.com/vi/a9_uHJhnKR4/mqdefault.jpg)](https://www.youtube.com/watch?v=a9_uHJhnKR4)

### コマンドラインで LND を操作する: lncli

LND は `bitcoin-lncli.sh` シェルスクリプトでコマンドラインから利用できます。
\
Docker 環境の場合は docker ディレクトリにいることを確認してください。

```bash
sudo su -
cd btcpayserver-docker
./bitcoin-lncli.sh $command
./bitcoin-lncli.sh getinfo #show info about the node
```

`./bitcoin-lncli.sh --help` を実行するとコマンド一覧を表示できます。詳細は [API documentation](https://api.lightning.community/) も参照してください。

## BTCPay Server と Core Lightning (CLN) の開始手順

### Ride The Lightning (RTL) で CLN を操作する

BTCPay Server で CLN 実装を使う最も簡単な方法は、**[Ride The Lightning](https://github.com/Ride-The-Lightning/RTL)** (RTL) サービスです。RTL は Lightning Network 用の Web UI で、ブラウザから BTCPay Server を離れずにノードを運用できます。
\
BTCPay Server で RTL を開始するには、Server Settings > Services > Ride The Lightning > See information へ進みます。

### コマンドラインで CLN を操作する: lightning-cli

`lncli` と同様に、CLN も `bitcoin-lightning-cli.sh` シェルスクリプトでコマンドラインから利用できます。
\
Docker 環境の場合は docker ディレクトリにいることを確認してください。

```bash
sudo su -
cd btcpayserver-docker
./bitcoin-lightning-cli.sh $command
./bitcoin-lightning-cli.sh getinfo #show info about the node
```

`./bitcoin-lightning-cli.sh help` を実行するとコマンド一覧を表示できます。詳細は [API documentation](https://lightning.readthedocs.io/) を参照してください。

## Lightning ノードのバックアップ

新しい lightning ノードで取引を始める前に、**on-chain** ウォレットのバックアップを検討してください。手順:

1. **LND の場合**: LND seed のコピーを保存。
   "Server Settings" > "Services" > "LND Seed Backup" > "See information" を選択
2. **CLN の場合**: [hsm_secret](https://lightning.readthedocs.io/BACKUP.html#hsm-secret) のコピーを保存
\
   CLN の $LIGHTNINGDIR は `/var/lib/docker/volumes/generated_clightning_bitcoin_datadir/_data/bitcoin` にあります。

**off-chain** payment channel バックアップの制約と関連リスクを理解してください。
\
Docker で BTCPay Server を運用している場合は [backup FAQ](https://docs.btcpayserver.org/Docker/backup-restore/#lightning-channel-backup) も参照してください。

### on-chain ウォレットへ資金を入れる

Lightning ノードが有効になったら、payment channel を開く前に on-chain ウォレットへ資金を入れる必要があります。
\
on-chain への資金投入は次の2つの方法で行えます。

1. Ride The Lightning (RTL) UI から行う

- "Store" を選択し、"Lightning" セクションへ移動
- "Services" の "Ride The Lightning" を選択
- RTL アプリで "On-chain" を開き、"On-chain Transactions" メニューの "Receive" を選択
- "Generate Address" を選び、資金送付先として使用

2. `bitcoin-lncli.sh` または `bitcoin-lightning-cli.sh` を使いコマンドラインで行う

```bash
sudo su -
cd btcpayserver-docker
./bitcoin-lncli.sh newaddress p2wkh #for LND
./bitcoin-lightning-cli.sh newaddr  #for CLN
{
   "address" / "bech32": "bc1..........." #use this as the destination for the allocated funds
}
```

on-chain lightning ノードへの資金投入が完了したら、ネットワーク上の他ノードへ接続して payment channel を開く段階です。
\
payment channel の開設、流動性管理などの推奨事項は [Payment channels](./LightningNetwork_PaymentChannels.md) を参照してください。

## Alby Extension

[Alby](https://getalby.com/) は、Bitcoin Lightning Network 上で通常のブラウザから簡単に Bitcoin 決済を送受信できる、無料で高速かつシンプルな手段です。BTCPay ウォレットを Alby アカウントへ直接接続できます。詳細は [how to connect your BTCPay wallet to Alby](https://guides.getalby.com/user-guide/v/alby-account-and-browser-extension/alby-lightning-account/connect-your-alby-lightning-account-to-other-apps/connect-to-btcpay-server) を参照してください。
