<!-- legacy-anchor-aliases -->
<span id="btcpay-server-keeps-showing-that-my-node-is-always-starting"></span>
<span id="btcpay-server-takes-forever-to-synchronize"></span>
<span id="can-i-skip-the-synchronization"></span>
<span id="how-can-i-check-the-block-height-of-my-bitcoin-node"></span>
<span id="how-do-i-know-that-btcpay-synced-completely"></span>
<span id="how-to-disable-bitcoin-node-pruning"></span>
<span id="how-to-enable-bitcoin-node-pruning"></span>
<span id="im-running-a-full-node-and-have-a-synched-blockchain-can-btcpay-use-it-so-that-it-doesnt-have-to-do-a-full-sync"></span>
<span id="why-does-btcpay-sync"></span>
<!-- /legacy-anchor-aliases -->

# 同期 FAQ

このドキュメントでは、BTCPay の同期中に発生しうる一般的な質問と問題をまとめています。

[[toc]]

## BTCPay が同期するのはなぜですか？

デプロイ後、BTCPay Server はブロックチェーン全体を同期し、すべてのコンセンサスルールを検証する必要があります。マシンスペック、帯域幅、追加したアルトコイン数によって、このプロセスは 1〜5 日かかることがあります。

手間に感じるかもしれませんが、誰にも依存せず信頼せずに自分のフルノードを運用するうえで重要なステップです。ノードは約 300GB のデータ（pruned node を使う場合はこれより少ない）をダウンロードするだけでなく、コンセンサスルールをすべて検証します。ブロックチェーン同期の重要性は [この動画](https://www.youtube.com/watch?v=OrYDehC-8TU) で詳しく説明されています。

BTCPay Server を学ぶことだけが目的で、独自インスタンスをデプロイせず単に [試したい](../TryItOut.md) 場合は、[Third-Party host](../Deployment/ThirdPartyHosting.md) を使えば同期を回避できます。

## 同期をスキップできますか？

BTCPay Server をデプロイする場合、同期はスキップできません。ただし、所要時間を大幅に短縮することはできます。コマンドライン操作に慣れているなら、FastSync を使ってノードをより高速に同期できます。この機能には信頼モデル上の注意点があるため、必ず [FastSync ドキュメント](https://github.com/btcpayserver/btcpayserver-docker/tree/master/contrib/FastSync) を確認してください。

FastSync を使うには、`opt-save-storage` 環境変数で [pruning を有効化](#how-to-enable-bitcoin-node-pruning) している必要があります。そうでないと bitcoind は同期できません。まず BTCPayServer インスタンスに [ssh 接続](./ServerSettings.md#how-to-ssh-into-my-btcpay-running-on-vps) し、次のコマンドを実行します。

```bash
sudo su -
cd $BTCPAY_BASE_DIRECTORY/btcpayserver-docker/
btcpay-down.sh
cd contrib/FastSync
./load-utxo-set.sh
# Once FastSync has completed
btcpay-up.sh
```

FastSync 完了後にインスタンスを再起動したら、BTCPay ドメインを更新して残りのブロックチェーン同期を待ちます。[この動画](https://youtube.com/watch?v=VNMnd-dX9Q8?t=1730) も参考になります。

FastSync 実行後に `You need to delete your Bitcoin Core wallet` が表示される場合、または `Last wallet synchronisation goes beyond pruned data` エラーが出る場合は、[BTCPay Server keeps showing that my node is always starting](#btcpay-server-keeps-showing-that-my-node-is-always-starting) の項目を参照してください。

## BTCPay が完全に同期されたことを確認する方法は？

右下に同期進捗ポップアップが表示されなくなれば、サーバーは完全同期済みで [利用開始](../RegisterAccount.md) できます。

BTCPay Server の Bitcoin ノードが最新ブロックまで同期しているか確認するには、任意のブロックチェーンエクスプローラーで現在のブロック高を確認し、[ノード高](#how-can-i-check-the-block-height-of-my-bitcoin-node) と一致するか比較してください。

## 自分の bitcoin ノードのブロック高を確認する方法は？

Bitcoin ノードの同期状態を確認するには、サーバーの Bitcoin コンテナ内で bitcoin-cli コマンドを使います。サーバーへ SSH 接続し、[Bitcoin ログを確認する](../Troubleshooting.md#23-bitcoin-node-logs) ディレクトリに移動したあと、`bitcoin-cli.sh getblockcount` を実行してサーバーの Bitcoin ノードの現在ブロック高を確認します。

## BTCPay Server の同期が非常に遅い

Bitcoin フルノードの同期には通常 1〜5 日かかります。最初は速く、終盤に近づくほど遅くなるのが一般的です。

同期していないように見える場合は、次を確認してください。

- CPU が不足している
- スワップメモリを使用している

### 原因 1: CPU が不足している

同期中は 2 CPU を推奨します。ただし、ホスティングプロバイダーによっては CPU 使用量が多いと制限されることがあります。

次で確認します。

```bash
sudo su -
docker stats
```

同期が非常に遅いのに CPU 使用率が 100% 超の場合:

```
8e7ac41e6e2a        btcpayserver_bitcoind               100%               560.5MiB / 3.853GiB   14.20%              4.17
```

この場合はマシンスペックを引き上げる必要があります。

同期中の CPU 使用率が非常に低い（10% 未満）場合:

```
8e7ac41e6e2a        btcpayserver_bitcoind               10%               560.5MiB / 3.853GiB   14.20%              4.17
```

ホスティングプロバイダー側で CPU 制限されている可能性があります。長時間の高 CPU 使用を許容しているか確認してください。

許容されない場合は、制限が解除されるまでサーバーを停止し、その後 docker 側で CPU を制限して再起動します。

```bash
docker update btcpayserver_bitcoind --cpus ".8"
```

### 原因 2: スワップメモリを使用している

同期時にメモリ不足だと、サーバーは動作継続のためスワップメモリを使用することがあります。

```bash
sudo su -
free -h
```

スワップ使用が確認できる場合:

```bash
              total        used        free      shared  buff/cache   available
Mem:           2.0G        2.0G        0M         66M        0G        0M
Swap:          1.0G        200M      800M
```

この場合はメモリを追加してサーバーをスケールアップしてください。

## BTCPay Server でノードが常に starting と表示される

考えられる原因:

- RAM が不足している
- ストレージが不足している
- 誤って pruning を無効化した
- bitcoin データディレクトリが破損している
- 最後のウォレット同期が pruned データ範囲を超えている

### 原因 1: RAM が不足している

RAM を確認します。

```bash
sudo su -
free -h
```

`free` がほぼない、または `available` が極端に少ない場合:

```bash
              total        used        free      shared  buff/cache   available
Mem:           2.0G        2.0G        0M         66M        0G        0M
Swap:            0B          0B          0B
```

メモリ増設が必要です。ノード同期済みであればスワップメモリを追加できます。未同期ならサーバースペックが不足しています。

同期済みの場合、次で 2G のスワップを追加できます。

```bash
fallocate -l 2G /mnt/swapfile
chmod 600 /mnt/swapfile
mkswap /mnt/swapfile
swapon /mnt/swapfile
echo "/mnt/swapfile   none    swap    sw    0   0" >> /etc/fstab
```

### 原因 2: ストレージが不足している

マシンのストレージを確認します。

```bash
sudo su -
df -h
```

ストレージ残量がない場合（例では `/dev/sda1`）:

```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            2.0G     0  2.0G   0% /dev
tmpfs           395M   41M  354M  11% /run
/dev/sda1       125G  125G  0G   100% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sdb1       7.8G   18M  7.4G   1% /mnt
```

維持したいストレージ量に応じた [docker fragment](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を選択し、その後 [ノードを prune](https://docs.btcpayserver.org/Docker/#how-i-can-prune-my-nodes) してください。

### 原因 3: 誤って pruning を無効化した

`export BTCPAYGEN_ADDITIONAL_FRAGMENTS="xyz"` で追加 fragment を設定した際、既存値を含め忘れると pruning を無効化してしまう場合があります。

Bitcoin ブロックチェーン全体を保存できるだけのメモリがなく、かつ実行中オプション一覧に `opt-save-storage` がない（[実行中オプションの確認](https://docs.btcpayserver.org/FAQ/Deployment/#how-can-i-modify-or-deactivate-environment-variables)）なら、pruning 無効化の可能性が高いです。

Bitcoind ログで確認できます。

```bash
sudo su -
cd btcpayserver-docker
docker logs --tail 100 btcpayserver_bitcoind
```

以下が表示される場合:

```bash
Block files have previously been pruned.
You need to rebuild the database using -reindex to go back to unpruned mode.
This will redownload the entire blockchain.
Please restart with -reindex or -reindex-chainstate to recover.
```

この場合は [pruning を再有効化](#how-to-enable-bitcoin-node-pruning) すれば解決できます。

### 原因 4: bitcoin データディレクトリが破損している

ノードログを確認します。

```bash
sudo su -
docker logs --tail 10 btcpayserver_bitcoind
```

以下が表示される場合:

```bash
Please restart with -reindex or -reindex-chainstate to recover.
```

bitcoin データディレクトリが破損しています。物理的なディスク損傷や故障の可能性があります。
ノードを再インデックスするには:

```bash
btcpay-down.sh
# Delete 'blocks' and 'chainstate' folders
rm -rf /var/lib/docker/volumes/generated_bitcoin_datadir/_data/blocks
rm -rf /var/lib/docker/volumes/generated_bitcoin_datadir/_data/chainstate
btcpay-up.sh
```

### 原因 5: 最後のウォレット同期が pruned データ範囲を超えている

FastSync を使った場合や、すでに同期済みのブロックチェーンを取り込んだ場合に発生することがあります。これは Bitcoin Core ウォレットを削除する必要がある状態で、原因は多くの場合、初回起動時に BTCPay Server が utxoset なしで起動し、ウォレットが utxoset より前に作成されたためです。該当ケースかどうかは [bitcoind ログ](../Troubleshooting.md#21-btcpay-logs) で次を確認します。

```bash
Error: Prune: last wallet synchronisation goes beyond pruned data. You need to -reindex (download the whole blockchain again in case of pruned node)
```

このエラーが表示され、同期完了のためにウォレット削除に同意する場合は、`btcpay-down.sh` 実行後、`btcpay-up.sh` 実行前に `docker volume rm generated_bitcoin_wallet_datadir` を実行してください。
WARNING: このウォレットに資金がある場合は削除しないでください。

## フルノードで同期済みブロックチェーンを持っています。フル同期せず BTCPay で使えますか？

可能です。ただし先に、docker volume を更新している既存の bitcoind を停止してください。その役割は BTCPay Server が引き継ぎます。

BTCPay Server を docker-compose で実行し、docker host 側に完全同期済みノードのデータディレクトリ（`.bitcoin`）がある場合、それを BTCPay Server で再利用できます。

手順は次のとおりです。

- [この手順](https://docs.btcpayserver.org/Docker/) に従って通常セットアップを行います。`opt-save-storage` 環境変数は pruning レベル有効化に使われます。既存データディレクトリを prune したくない場合は、BTCPay docker デプロイで `export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-save-storage-s"` を設定しないでください。
- `btcpay-setup.sh` 完了後、`btcpay-down.sh` で docker compose を停止します。
- `sudo su -` で root ログインします。
- bitcoind の docker volume を開きます: `cd /var/lib/docker/volumes/generated_bitcoin_datadir/`。`ls -la` で内容を確認すると `_data` ディレクトリのみのはずです。
- `_data` ディレクトリを削除します: `rm -r _data`。保持したい場合は `mv _data/ _data.old/` でリネームします。
- `/var/lib/docker/volumes/generated_bitcoin_datadir/_data` と host 上のデータディレクトリ（`.bitcoin`）の間に [シンボリックリンク](https://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/) を作成します: `ln -s path/to/.bitcoin /var/lib/docker/volumes/generated_bitcoin_datadir/_data`
- `ls -la` でリンク作成を確認します。
- `btcpay-up.sh` で docker-compose を再起動します。

これで BTCPay Server は完全同期済みになるはずです。

この後も BTCPay Server がノードを常に starting と表示する場合は、[BTCPay Server keeps showing that my node is always starting](#btcpay-server-keeps-showing-that-my-node-is-always-starting) の原因を参照してください。

## Bitcoin ノードの pruning を有効化するには？

これにより Bitcoin フルノードを最大 100GB（blocks）まで pruning できます。

```bash
sudo su -
cd btcpayserver-docker
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-save-storage"
. ./btcpay-setup.sh -i
```

他の pruning オプションは [こちら](https://docs.btcpayserver.org/Docker/#generated-docker-compose) を参照してください。他の additional fragments と併用する場合は [この例](./Deployment.md#how-can-i-modify-or-deactivate-environment-variables) を参照してください。

## Bitcoin ノードの pruning を無効化するには？

BTCPay で Bitcoin ノードの pruning を無効化する前に、システムにブロックチェーン全体と BTCPayServer を保存できるだけのメモリがあることを確認してください。そのうえで `opt-save-storage` 環境変数を無効化します。fragment 一覧の確認と 1 つだけ削除する方法は [この例](./Deployment.md#how-can-i-modify-or-deactivate-environment-variables) を参照してください。次の例は additional fragments を **すべて** 削除します。

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS=""
. ./btcpay-setup.sh -i
```

その後、pruning なし Bitcoin ノードを再作成するため次のコマンドを実行します。

```bash
btcpay-down.sh
# Delete 'blocks' and 'chainstate' folders
rm -rf /var/lib/docker/volumes/generated_bitcoin_datadir/_data/blocks
rm -rf /var/lib/docker/volumes/generated_bitcoin_datadir/_data/chainstate
btcpay-up.sh
```
