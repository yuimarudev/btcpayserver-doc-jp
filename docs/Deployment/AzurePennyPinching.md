# Azure デプロイを節約運用する方法

このガイドは、インストール費用を節約したい [Azure デプロイ](https://github.com/btcpayserver/btcpayserver-azure) のユーザー向けです。

これは **ノードの同期が完全に終わった後にのみ** 実施してください。
同期中は高性能な構成が必要です。

**節約運用**は、消費しているリソースを理解し、ワークロードに合わせて構成を調整する良い機会です。

デメリット:

- `btcpay-update.sh` の実行や再起動に時間がかかる
- `502 Bad Gateway` が表示され、ノード起動に非常に時間がかかる場合がある
- サーバーが非常に遅くなる可能性がある

メリット:

- 50% の節約

サーバーが遅すぎる場合:

- `/etc/profile.d/btcpay-env.sh` の `BTCPAY_DOCKER_COMPOSE` を編集して対応コインを減らす、または
- Virtual Machine のサイズを上げる

:::warning
いくつかのテストの結果、BTC+LTC+CLightning を含む mainnet 構成でこのガイドを適用するのはやや負荷が高く、サーバーがかなり重くなるようです。

再起動後、時間の経過とともにサーバーは軽くなるため、ケースによっては問題ない可能性もあります。
許容できない場合は、`B1MS`（20 USD/月）から `B2S`（40 USD/月）へ切り替えるべきです。
:::

## 今どれくらい支払っているか？

まず、現在のインストールコストを確認します。

- Azure ポータルへ移動
- Subscription へ移動（`Subscription` メニューが見つからない場合は、通知ベル横の検索バーで `Subscription` を検索）
- Cost Analysis へ移動
- Resource group を選択（私の環境では "dwoiqdwqb'" という名前）
- 期間を 30 days に設定
- apply をクリック

![Show Cost Microsoft Azure](../img/ShowCost.png)

この例では、インストールコストは `47.00 EUR/Month` です。
大部分のコストは virtual machine に使われています。

## 現在の構成は何か

まず、現在どの Virtual machine を使っているかを確認します。

- Azure ポータルへ移動
- Resource Groups へ移動
- あなたの resource group を選択
- BTCPayServerVM を選択

![Show Microsoft Azure VM](../img/ShowVM.png)

この例では、CPU とディスクは主に未使用です。ここは削減できそうです。
また VM タイプは `Standard_D1_v2` です。[Azure Price Website](https://azureprice.net/) で確認できます。

![Show Azure Price](../img/ShowPrice.png)

この VM は `0.0573444 EUR/H`、つまり `42.66 EUR/Month` かかります。

つまり、この VM をダウングレードすることが最大のコスト削減効果になります。
どこまで下げられるか確認しましょう。

SSH で VM に接続し、次を実行します。

```bash
sudo su -
docker stats
```

![Show Azure Resources](../img/ShowResources.png)

この例では、RAM は 3.352 GB で、使用率は約 55% です。

`free` コマンドからも、約 1GB 程度は余剰があると見て取れます。

```
root@BTCPayServerVM:~# free --human

             total       used       free     shared    buffers     cached
Mem:          3.4G       3.2G       138M        30M       8.8M       991M
-/+ buffers/cache:       2.2G       1.1G
Swap:           0B         0B         0B
```

## 新しい Virtual Machine を選ぶ

ここまでで、RAM 2 GB とより低性能な CPU でもおそらく動くと分かりました。

ただし先に、RAM 不足でマシンが落ちないように swap を追加します。
Azure では `/mnt` が一時データ用で低レイテンシに最適化されているため、swapfile をここに作成します。

```bash
sudo su -
fallocate -l 2G /mnt/swapfile
chmod 600 /mnt/swapfile
mkswap /mnt/swapfile
swapon /mnt/swapfile
echo "/mnt/swapfile   none    swap    sw    0   0" >> /etc/fstab
```

次のように、swap が追加されたことを確認できます。

```
root@BTCPayServerVM:~# free -h
             total       used       free     shared    buffers     cached
Mem:          3.4G       3.2G       141M        30M       9.8M       983M
-/+ buffers/cache:       2.2G       1.1G
Swap:         **2.0G**         0B       2.0G
```

次に [azureprice.net](https://azureprice.net/) へ戻り、`0.0573444 EUR/H` より安いものを探します。

![Azure VM comparison](../img/ShowB1.png)

`Standard_B1ms` は `0.02049219 EUR/H` しかかかりません。これに切り替えましょう。

[この記事](https://www.singhkays.com/blog/understanding-azure-b-series/)をざっと見ると、この VM タイプは低い CPU 消費と一時的なバーストに適していることが分かります。ノード同期後の BTCPay Server には適した特性です。

- Azure ポータルへ移動
- Resource Groups へ移動
- あなたの resource group を選択
- BTCPayServerVM を選択
- `Size` を選択
- `B1MS` を選択（表示されない場合は [FAQ](#b1ms) を確認）
- `Select` をクリック

![Show Azure VM Size](../img/ShowSize.png)

5〜15 分待ちます。

Azure の処理が完了すると:

![Happy Microsoft Azure](../img/HappyAzure.png)

完了です。月額コストを 50% 削減できました。

### FAQ: 一覧に B1MS が表示されない <a name="b1ms"></a>

状況によっては、一覧に B1MS Virtual Machine が表示されないことがあります。
これは、あなたが利用している Azure ハードウェアクラスターがこのタイプをサポートしていないことを意味します。

:::warning
Virtual Machine を停止すると、サーバーの公開 IP アドレスは変更されます。ドメインレジストラで CNAME ではなく A レコードを設定している場合は、更新が必要です。
:::

次の操作を行います。

- Virtual Machine リソースを開く
- `Overview` メニューへ移動
- `Stop` をクリック

![Stop Azure VM](../img/StopVM.png)

Virtual Machine が停止するまで待ってから、サイズを変更します。

サイズ変更後は `Overview` に戻り、`Start` をクリックします。
