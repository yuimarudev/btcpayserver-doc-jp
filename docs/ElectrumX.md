# ElectrumX

このドキュメントでは、**Electrum Wallet を ElectrumX Server に接続する方法**を説明します。

**注:** [BTCPay Server の Docker 版](https://github.com/btcpayserver/btcpayserver-docker)（2019年11月7日以降、version 1.0.3.137）は [ElectrumX](https://electrumx.readthedocs.io/en/latest/features.html) との完全統合をサポートしています。ElectrumX は、ローカルの Electrum ウォレットが Bitcoin ブロックチェーンの詳細取得やトランザクションのブロードキャストに使う公開 Electrum サーバーで最も広く利用される実装です。意味とセットアップ方法は以下の Section 2 を参照してください。

## ElectrumX を BTCPay Server に統合し、Electrum Wallet を接続してプライバシーを高める

### （BTCPay の Docker 版のみ対応）

進める前に、PC/Mac 上の Electrum ウォレットが、独自の Bitcoin フルノードなしで高速に動作する仕組みを理解しておくことが重要です。実際には、Electrum Wallet は世界中のコミュニティ運営サーバー（Bitcoin フルノード付き）に依存しています。これらは Electrum Servers と呼ばれます。あなたもそのネットワークを強化する側に回れます。

Electrum Wallet の下部にある小さな信号アイコンをクリックすると:

![ElectrumWalletMainScreenLight](https://user-images.githubusercontent.com/1388507/68437133-5636c500-01c0-11ea-822c-6e72bd6d60ea.png)

通常は `Select Server Automatically` が有効になっており、接続可能な Electrum サーバー一覧が表示されます:

![ElectrumWalletServerList](https://user-images.githubusercontent.com/1388507/68437521-8a5eb580-01c1-11ea-9ece-0666353a6742.png)

`Select Server Automatically` を有効にしたまま使うのが最も簡単ですが、Electrum Wallet で行う取引の閲覧やブロードキャストは他者のサーバー経由になります。これはプライバシー上のリスクであり、独自の ElectrumX Server を運用することで軽減できます。

## Section 2.1 独自 ElectrumX Server を有効化する（BTCPay Server の Bitcoin フルノードと完全統合）

### 前提条件（必須）

1. Docker のみ: 対応するのは [BTCPay Server の Docker 版](https://docs.btcpayserver.org/Docker/)のみです。
2. 非プルーニングの BTCPay ノード: BTCPay 実装が [pruned](./FAQ/Synchronization.md#can-i-skip-the-synchronization) でないことを確認してください（つまり genesis block から同期・保存されていること）。`opt-save-storage` の [Environment Variable](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を使っていないことを確認してください。
3. ディスク容量: Docker ボリューム保存先のデバイスに最低 400GB が必要です（このドキュメント執筆時点の2019年11月9日で、フルノード + ElectrumX 有効時に 333GB 使用。今後さらに増加します）。
4. Additional Fragments: 環境変数設定で使う BTCPay の [Additional Fragment](https://docs.btcpayserver.org/Docker/#environment-variables) 機能を理解していること。
5. サーバーアーキテクチャ: ここで使用する（公式）[ElectrumX docker](https://github.com/lukechilds/docker-electrumx) は、x86_64 で動作する BTCPay Server でのみテストされています。現時点では Ubuntu 18.04 と Debian Buster で広く検証済みです。Raspberry Pi（および他アーキテクチャ）向けには十分な検証が行われるまで動作しない可能性があります。
6. Linux コマンドラインの基本知識があること。

### ElectrumX Server の有効化は既存の BTCPay 実装にどう影響するか

基本的に、BTCPay Server で ElectrumX を設定する作業はシンプルで、他の実装部分には影響しません。必要条件は上記のみです。[ElectrumX 公式 docker リリース](https://github.com/lukechilds/docker-electrumx) は、[`opt-add-electumx`](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/docker-fragments/opt-add-electrumx.yml) という [additional fragment](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を有効化することで BTCPay で動作します。この fragment は ElectrumX サーバーを有効化・起動するだけでなく、Bitcoin フルノードで `txindex=1` も有効化します。`txindex=1`（Transaction Index=ON）は、ElectrumX が任意の取引の詳細データをサードパーティ経由なしでブロックチェーンから直接提供するために必要な Bitcoin Core の機能です。

すでに BTCPay Server を運用していて、これまで `txindex=1` を有効にしていなかった場合、インデックス構築に数時間かかることがあります。通常これは問題なく、ダウンタイムも数時間以内で済むはずですが、ノード利用者が少ない夜間に実行することを推奨します。注: インデックスを最初から再構築したい場合は、`reindex=1` オプション付きで bitcoind を一度起動してください（警告: `reindex` は非常に長時間かかる可能性があり、通常は不要なためデフォルトでは有効化されておらず、このドキュメントの対象外です）。

### BTCPay で ElectrumX Server を有効化する手順

以下は **BTCPay ノードで ElectrumX Server を有効化する手順**です。特に、他のカスタムまたは競合する "fragments"（pruning、less-memory など）を使っている場合は、環境に合わせて調整が必要です。繰り返しになりますが、pruned な BTCPay ノードを使用している場合はこの先に進まないでください。

1. ElectrumX Server は TCP ポート 50002 経由で Electrum Wallet からアクセスされます。少なくともローカルネットワーク内の Electrum Wallet（PC または Android）から利用できるよう、このポートを開放してください。必要であればインターネット/WAN 側からの 50002 ポートフォワーディングも設定できます（外部ユーザーがあなたのサーバーを参照可能になります）。

2. 次のコマンドで BTCPay ノードの Docker Additional Fragment を有効化します（以下は LND と ElectrumX を含む新規 BTCPay インストールを想定。必要に応じて [関連ドキュメント](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を参照して調整してください）。

3. まず [通常の BTCPay Server セットアップ手順](https://github.com/btcpayserver/btcpayserver-docker#full-installation-for-technical-users) に従って進め、`cd btcpayserver-docker` 実行後はリンク先の続きではなく以下の手順を実施してください。すでに BTCPay Server を運用中なら次のステップからで構いません。

4. 環境変数を設定します:

```bash
export BTCPAY_HOST="YOURHOST.com"
export NBITCOIN_NETWORK="mainnet"
export BTCPAYGEN_CRYPTO1="btc"
export BTCPAYGEN_REVERSEPROXY="nginx"
export BTCPAYGEN_LIGHTNING="lnd"
export LIGHTNING_ALIAS="MY_LN"
export LETSENCRYPT_EMAIL="you@example.com"
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-add-electrumx;opt-more-memory"
```

必要に合わせて調整すれば、上記は1コマンドとしてまとめて実行できます。このガイドで最重要なのは `BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-add-electrumx"` です。注: `opt-more-memory` は削除可能ですが、BTCPay Server に 1GB 超の RAM を割り当てられる環境なら有効化を強く推奨します。ノード同期と ElectrumX の全体パフォーマンスが大きく向上します。

5. ElectrumX 付きで BTCPay Server をセットアップまたは再設定します:

   `cd ~/BTCPayServer/btcpayserver-docker && . ./btcpay-setup.sh -i`

   これで ElectrumX を含む必要な構成がセットアップ（または再セットアップ）され、通常はそのまま動作します。ただし `txindex` の同期に最低でも数時間かかります。新規サーバーの場合はハードウェア次第で数日かかることもあります。

6. ノードの完全同期を待ちます:
   Bitcoin Core 同期状態は BTCPay のドメイン先フロントページで確認できます。コマンドラインでは次を使います。

   `docker logs btcpayserver_bitcoind` - Bitcoin Core のブロックチェーン同期状態（およびエラーを含むノード情報全体）を表示します。

   `docker logs generated_electrumx_1` - ElectrumX の同期状態を表示します。注: ElectrumX は Bitcoin フルノードの同期完了まで同期を開始しません。完了前のエラー表示は無視できます。

Bitcoin と ElectrumX の双方の同期が完了したら次のステップへ進みます。（注: 同期未完了の Electrum サーバーには Electrum Wallet は接続できません）

## Section 2.2 Electrum Wallet（Desktop または Android）を ElectrumX Server に接続する

### Mac/PC/Linux の Electrum Wallet から ElectrumX に接続する

先に全体を読んでから進めてください。Electrum Wallet GUI で手動設定する代わりに、以下の "Protip" だけ実施しても構いません。

Electrum Wallet を開き、下部の信号アイコンをクリックします:

![ElectrumWalletMainScreenLight](https://user-images.githubusercontent.com/1388507/68437133-5636c500-01c0-11ea-822c-6e72bd6d60ea.png)

通常は `Select Server Automatically` が有効になっており、接続可能な Electrum サーバー一覧が表示されます:

![ElectrumWalletServerList](https://user-images.githubusercontent.com/1388507/68437521-8a5eb580-01c1-11ea-9ece-0666353a6742.png)

ここで `Select Server Automatically` をオフにすると、ElectrumX Server の IP アドレス、ドメイン、またはホスト名を入力できるようになります。以下の例ではローカルネットワーク上の ElectrumX サーバー `192.168.1.3` を手動入力し（ポートは 50002 のまま）、`close` を押します。

![EnterElectrumXServerIP](https://user-images.githubusercontent.com/1388507/68496320-4e276580-0252-11ea-8caf-facc8a246d70.png)

上記が正しく完了し、ノード状態が正常であれば、画像のようにウォレット画面右下の信号が緑になります。これで成功です。

![ElectrumWalletMainScreenLight](https://user-images.githubusercontent.com/1388507/68437133-5636c500-01c0-11ea-822c-6e72bd6d60ea.png)

#### Protip - Wallet GUI を開く前に Electrum Wallet の config ファイルを直接設定する方法

Electrum Wallet 起動直後に他サーバーへ接続したくない場合は、GUI を開く前に次を実施してください。

Electrum Wallet フォルダ（場所が不明な場合は[こちら](https://electrum.readthedocs.io/en/latest/faq.html#where-is-my-wallet-file-located)）で、`config` ファイルを開いて以下のように編集します。

1. `"auto_connect": true,` を `"auto_connect": false,` に変更します。これにより起動時にサードパーティの Electrum サーバーへ自動接続しなくなります（ブロックヘッダーや取引情報取得のための接続を防止）。

2. `"oneserver": false,` を `"oneserver": true,` に変更します。すべてのデータ取得を単一サーバーに固定できます。

3. `"server": "SOMEIPADDRESS:50002:s",` を探すか追加し、自分の ElectrumX Server の IP に変更します。上記例なら `"server": "192.168.1.3:50002:s",` です。Wallet 起動時のデフォルト接続先として固定されます。

この3ステップは任意ですが、Electrum Wallet の接続先を自分のプライベートサーバー1台に固定して完全なプライバシーを得るために推奨されます（[Reference](https://github.com/chris-belcher/electrum-personal-server#how-to)）。

### 達成できたこと

これで **自分専用のプライベート ElectrumX Server** を運用しています。Electrum Wallet 関連のデータ転送は、他のサードパーティサーバーを経由せず、ElectrumX Server と Bitcoin ブロックチェーンの間で直接行われます。少なくともブロックチェーンクエリ、取引、受取/支払アドレスの観点で、Bitcoin トランザクションのプライバシーを高いレベルで確保できます。

### トラブルシューティング

手順どおりに設定しても、上記ステップで赤信号（どのサーバーにも未接続）になる場合があります。ほかの有用なトラブルシュート情報があれば、このドキュメントに PR を出す形で追加してください。

- 赤信号のままの場合は Electrum Wallet を完全終了し、Electrum Wallet フォルダへ移動します（場所が不明な場合は[こちら](https://electrum.readthedocs.io/en/latest/faq.html#where-is-my-wallet-file-located)）。

Electrum Wallet フォルダ内（以下は Mac の例）で `certs` ディレクトリを開き、接続先サーバーの証明書（この例では `192.168.1.3`）を見つけてゴミ箱へ移動し削除します。

![Certs](https://user-images.githubusercontent.com/1388507/68497330-9a73a500-0254-11ea-9349-71bdb3bd9511.png)

Electrum Wallet を再起動して ElectrumX サーバーに接続します。サーバー同期が完了していれば、信号は緑になり利用可能です。
