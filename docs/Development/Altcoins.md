# Altcoin の追加方法

BTCPay Server プロジェクトとそのコア開発者は Bitcoin のみに注力しています。ただし、任意で有効化できる altcoin 統合がいくつか存在します。詳細は [Altcoin FAQ page](../FAQ/Altcoin.md) を確認してください。

## 概要

Altcoin 統合には次の2点が必要です。

1. **BTCPay Server plugin** - 決済ロジック、ウォレット管理、UI を担当
2. **Docker Compose fragment** - 他のユーザーが BTCPay Server と並行して対象コインのノード基盤を実行できるようにする

BTCPay Server のコアを変更してはいけません。新しいコインはすべてプラグインとして統合する必要があります。

## BTCPay Server プラグインを作成する

`BaseBTCPayServerPlugin` を拡張するプラグインを作成します。プラグインでは、コインのネットワーク、支払いハンドラ、UI 拡張を BTCPay Server に登録します。

参考として [Monero plugin](https://github.com/btcpay-monero/btcpayserver-monero-plugin) を使用してください。確認すべき主要ファイルは次のとおりです。

* [MoneroPlugin.cs](https://github.com/btcpay-monero/btcpayserver-monero-plugin/blob/master/Plugins/Monero/MoneroPlugin.cs) - プラグインのエントリポイントとサービス登録
* [MoneroLikePaymentMethodHandler.cs](https://github.com/btcpay-monero/btcpayserver-monero-plugin/tree/master/Plugins/Monero/Payments) - 支払い処理
* [MoneroListener.cs](https://github.com/btcpay-monero/btcpayserver-monero-plugin/tree/master/Plugins/Monero/Services) - トランザクションとブロックの監視
* [MoneroLoadUpService.cs](https://github.com/btcpay-monero/btcpayserver-monero-plugin/tree/master/Plugins/Monero/Services) - RPC 経由でのウォレット作成と読み込み

一般的なプラグイン開発手順（プロジェクト設定、UI 拡張、データベース、公開方法）については [Plugin Development docs](./Plugins.md) を参照してください。

## Docker Compose Fragment を作成する

ユーザーが BTCPay Server と一緒に対象コインのノードをデプロイできるようにするには、次の構成要素を含む PR を [btcpayserver-docker](https://github.com/btcpayserver/btcpayserver-docker) リポジトリへ提出してください。完全な例は [Beldex PR #1042](https://github.com/btcpayserver/btcpayserver-docker/pull/1042) を参照してください。

### Docker image

対象コインの daemon と wallet RPC 用の Docker イメージをビルドして公開します。公開先は自身の Docker Hub またはコンテナレジストリにし、BTCPay のイメージビルドパイプラインへは追加しないでください。

### Docker Compose fragment

`docker-compose-generator/docker-fragments/` に YAML ファイルを追加し、対象コインの daemon と wallet RPC サービスを定義します。プラグインが接続できるよう、この fragment で RPC URI を環境変数経由で BTCPay Server に渡す必要があります。

例: [monero.yml](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/docker-fragments/monero.yml)

### crypto-definitions.json

docker-compose generator に取り込まれるよう、[`docker-compose-generator/crypto-definitions.json`](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/crypto-definitions.json) に対象コインのエントリを追加します。

### 任意の追加要素

必要に応じて次も追加できます。

* **Expose fragment**（例: `opt-yourcoin-expose.yml`）- デバッグ用に localhost で RPC ポートを公開
* **Wallet CLI scripts**（例: `yourcoin-wallet-cli.sh`）- コンテナ内の wallet CLI へアクセスするための補助ラッパー
* **Backup script changes** - `btcpay-backup.sh` に対象コインのボリュームを追加

## 公開と掲載

統合の準備ができたら、次を実施します。

1. テストユーザーが BTCPay Server UI からインストールできるよう、[BTCPay Plugin Builder](https://plugin-builder.btcpayserver.org/) に **プラグインを公開** します（ユーザーは、あなたがプラグインを開発した BTCPay Server リポジトリの fork を実行している必要があります）。
2. ユーザーが対象コインのノードをデプロイできるよう、[btcpayserver-docker](https://github.com/btcpayserver/btcpayserver-docker) への **Docker PR をマージ** してもらいます。
3. 前ステップの Docker PR がマージされた後にのみ、BTCPay Plugin Builder への **掲載申請** を行います。

## FAQ

### ローカルでプラグインをテストするには？

プラグインのリポジトリには、対象コインの daemon（regtest モード）、wallet RPC、BTCPay の基本依存関係（bitcoind、nbxplorer、postgres）を起動する専用の `docker-compose.yml` を含めるべきです。そのうえで、`DEBUG_PLUGINS` 経由でプラグインを読み込んだ状態で、IDE から BTCPay Server を実行します。

完全なワークフローは [Monero plugin's docker-compose.yml](https://github.com/btcpay-monero/btcpayserver-monero-plugin/blob/master/BTCPayServer.Plugins.IntegrationTests/docker-compose.yml) と [Local Development Setup](https://github.com/btcpay-monero/btcpayserver-monero-plugin#local-development-setup) を参照してください。

### 本番向けに Docker fragment をテストするには？

[.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) をインストールして次を実行します。

```bash
BTCPAYGEN_CRYPTO1='YOUR-COIN'
BTCPAYGEN_SUBNAME='test'
cd docker-compose-generator/src
dotnet run
```

これで `Generated` フォルダに docker-compose が生成されます。fragment が正しく含まれていることを確認してください。

## メンテナンス

BTCPay 開発者は、依頼ベースで代替コインの実装は行いません。新しいコインの追加は、そのコインのコミュニティと開発者に完全に依存します。BTCPay 開発者は altcoin のテストや保守に多くの工数を割きません。PR を提出する場合は、統合が正しく動作することを必ず確認してください。アクティブに保守されない場合は削除されます。
