# Raspberry Pi デプロイ

このドキュメントでは、**Raspberry Pi 4 で BTCPay Server を実行する方法**をステップごとに案内します。

現在入手できる低価格のシングルボードコンピューターの中では、**Raspberry Pi 4** が最も優れています。
以下で説明するパーツをそろえれば、約150ドルで **Raspberry Pi 4 を使って自宅で BTCPay Server を運用**できます。

すでに次の仕様の Raspberry Pi 4 をお持ちですか？

- 4GB メモリ
- 1TB USB 3.0 SSD
- 16GB 以上の SD カード

お持ちであれば、すぐに [quickstart instructions](#quickstart) に進めます。
そうでなければ、必要なものは次のとおりです…

## 必要なハードウェア

### Raspberry Pi 4

- [**4GB RAM** 搭載 Raspberry Pi 4](https://www.canakit.com/raspberry-pi-4-4gb.html) (~$65)
- [Sandisk 16GB SD Card](https://www.amazon.com/dp/B073K14CVB/) (~$10)

RAM が 1GB や 2GB のモデルで妥協しないでください。**4GB RAM** 版は他の版より見つけにくいことがありますが、数ドルの差で **4GB RAM** を選ぶ価値は十分あります。4GB RAM 版の在庫がある販売店を見つけるために、少し時間をかけて探すことを強くおすすめします。まだ持っていない場合は **SD カードリーダー** も必要です。

### データ保存オプション

- [Samsung SSD T7 1TB](https://www.amazon.com/dp/B0874XN4D8/) (~$100)
- [SanDisk Ultra 3D 1TB](https://www.amazon.com/dp/B071KGRXRG/) (~$100)

1TB の SSD があれば、Bitcoin ブロックチェーンの完全なコピーを保持できます。
また、[pruning option](/Docker/#how-i-can-prune-my-nodes) を使えば、Bitcoin ブロックチェーンの完全コピーがなくても BTCPay Server を利用できます。

### 電源アダプターのオプション

- [Official Raspberry Pi 4 USB-C Power Adapter 5.1V/3A for US](https://shop.pimoroni.com/products/raspberry-pi-official-usb-c-power-supply-us?variant=29391144648787) ($10)
- [Official Raspberry Pi 4 USB-C Power Adapter 5.1V/3A for EU](https://shop.pimoroni.com/products/raspberry-pi-official-usb-c-power-supply-eu?variant=29391130624083) ($10)
- [Official Raspberry Pi 4 USB-C Power Adapter 5.1V/3A for AU](https://shop.pimoroni.com/products/raspberry-pi-official-usb-c-power-supply-au?variant=29391160737875) ($10)

Amazon の安価な無名アダプターに時間を使ったり、家にある既存のアダプターで問題ないと考えたりしないでください。Raspberry Pi 4 は非公式アダプターで問題が出ることがあり、10ドル程度なら、痛い目を見る前に **公式アダプターを購入**するほうが確実です。

### ケースと冷却オプション

- [Flirc Heatsink Case](https://www.amazon.com/dp/B07WG4DW52/) (~$15)
- [Passive cooling aluminum case](https://www.amazon.com/dp/B07VQRYTPR/) (~$15)

もちろんケースの使用は完全に任意ですが、Raspberry Pi を長期的に保護するために推奨します。
厳密には冷却機構が必須というわけではありませんが、少なくとも受動冷却は **あったほうがよい**です。
Raspberry PI のコア温度が 70°C に達すると、CPU がサーマルスロットリングされます。

## Quickstart

最新の [Raspberry Pi Imager](https://www.raspberrypi.com/software/) をダウンロードして起動します。

![Raspberry Pi Imager](../img/raspberry-pi/rpi-imager.png)

次のオプションを選択します。

- Operating System（OS）: Raspberry Pi OS Lite (64-bit) を選択
  - "Raspberry Pi OS (Other)" から選択
- Storage（ストレージ）: SD カードを選択

右下のボタンから Advanced Settings（詳細設定）を開きます。

![Raspberry Pi Imager Advanced Settings](../img/raspberry-pi/rpi-imager-advanced-settings.png)

Advanced Settings:

- ホスト名を設定します。このガイドでは `btcpay.local` を想定します。
- SSH を有効化
- ユーザー名とパスワードを設定します。このガイドではユーザー名を `btcpay` と想定します。

その他の設定は任意です。無線 LAN を設定する必要はありません。

Advanced Settings を閉じて、"Write" ボタンをクリックします。

### Raspberry Pi のセットアップ

イメージの書き込みが SD カードに完了したら、SD カードを取り外して Raspberry Pi に挿入します。
Raspberry Pi に SSD とネットワークケーブルを接続します。
最後に電源ケーブルを接続すると、起動プロセスが始まります。
起動後、DHCP により IP アドレスが割り当てられるはずです。

Raspberry Pi Imager で設定した認証情報で Raspberry Pi にログインします。

```bash
ssh btcpay@btcpay.local
```

`Are you sure you want to continue connecting?` と聞かれたら `yes` を入力します。

`btcpay.local` で Raspberry Pi が見つからない場合は、ルーターにログインして IP アドレスを確認する必要があります。
私の Raspberry Pi では `192.168.1.5` が割り当てられました。

```bash
ssh btcpay@192.168.1.5
```

`root` ユーザーに切り替えます。

```bash
sudo su -
```

その後、Lightning ノードとして [LND](https://github.com/lightningnetwork/lnd) か [Core Lightning](https://github.com/ElementsProject/lightning) のどちらかを選べます。

**必須:** 次のどちらかを選択してください…

```bash
# Core Lightning
export BTCPAYGEN_LIGHTNING="clightning"

# LND
export BTCPAYGEN_LIGHTNING="lnd"
```

**任意:** [additional settings](/Docker/#environment-variables) も設定できます…

```bash
# optional, this is just an example for runing a pruned node on a public domain
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-save-storage"
export BTCPAY_ADDITIONAL_HOSTS="btcpay.YourDomain.com"
```

インストールスクリプトをダウンロードして実行します。

```bash
wget -O btcpayserver-install.sh https://raw.githubusercontent.com/btcpayserver/btcpayserver-doc/master/scripts/btcpayserver-rpi4-install.sh
chmod +x btcpayserver-install.sh
. btcpayserver-install.sh
```

初期セットアップが完了したら、別のコンピューターのブラウザーで `btcpay.local` にアクセスします。

:::tip
インストールは完了しており、ノードの同期が開始されているはずです。
Bitcoin フルノードの場合、インストール後の初回ブロックダウンロードにはおおよそ 40 時間かかります。
:::

参考までに、上記のインストールスクリプトが実行している内容の詳細を以下に示します…

## 詳細なステップバイステップ手順

以下は [quickstart instructions](#quickstart) で示した一般的なセットアップの後に行う手順です。

:::tip NOTE
以下の手順では `root` ユーザーである必要があります。

```bash
sudo su -
```

:::

### OS パッケージを最新化

```bash
apt update && apt upgrade -y && apt autoremove
```

### ストレージ設定

SD カードの消耗を防ぐため、swap を無効化することを推奨します。

```bash
dphys-swapfile swapoff
dphys-swapfile uninstall
update-rc.d dphys-swapfile remove
systemctl disable dphys-swapfile
```

SSD をパーティション分割します。

```bash
fdisk /dev/sda
# type 'p' to list existing partitions
# type 'd' to delete currently selected partitions
# type 'n' to create a new partition
# type 'w' to write the new partition table and exit fdisk
```

SSD 上の新しいパーティションをフォーマットします。

```bash
mkfs.ext4 /dev/sda1
```

起動時に SSD パーティションが自動マウントされるよう設定します。

```bash
mkfs.ext4 /dev/sda1
mkdir /mnt/usb
UUID="$(sudo blkid -s UUID -o value /dev/sda1)"
echo "UUID=$UUID /mnt/usb ext4 defaults,noatime,nofail 0 0" | sudo tee -a /etc/fstab
mount -a
```

`/etc/fstab` を編集するついでに、ログ用 RAM ファイルシステムを追加できます（任意）。
これも SD カードの早期消耗を防ぐためです。

```bash
echo 'none /var/log tmpfs size=10M,noatime 0 0' >> /etc/fstab
```

### Docker のインストール

```bash
apt install apt-transport-https ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
apt update
apt -y install docker-ce docker-ce-cli containerd.io
```

### Docker ボリューム用マウントの作成

```bash
rm -rf /var/lib/docker
mkdir -p /var/lib/docker
mount --bind /mnt/usb /var/lib/docker
echo "/mnt/usb /var/lib/docker none bind,nobootwait 0 2" >> /etc/fstab
systemctl restart docker
```

### ファイアウォール設定

ファイアウォールをインストールし、SSH、HTTP、HTTPS、Bitcoin、Lightning を許可します。

```bash
apt install -y ufw
ufw default deny incoming
ufw default allow outgoing
```

次のコマンドは、内部ネットワークからの SSH 接続のみを許可します。

```bash
ufw allow from 10.0.0.0/8 to any port 22 proto tcp
ufw allow from 172.16.0.0/12 to any port 22 proto tcp
ufw allow from 192.168.0.0/16 to any port 22 proto tcp
ufw allow from 169.254.0.0/16 to any port 22 proto tcp
ufw allow from fc00::/7 to any port 22 proto tcp
ufw allow from fe80::/10 to any port 22 proto tcp
ufw allow from ff00::/8 to any port 22 proto tcp
```

次のポートはどこからでもアクセス可能にする必要があります（サブネットを指定しない場合、既定は `any` です）。

```bash
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 8333/tcp
ufw allow 9735/tcp

# Enable the firewall
ufw enable

# Verify the configuration
ufw status
```

### BTCPay Server のセットアップ

GitHub から BTCPay Server をダウンロードします。

```bash
cd # ensure we are in root home
apt install -y fail2ban git
git clone https://github.com/btcpayserver/btcpayserver-docker
cd btcpayserver-docker
```

いくつかの [environment variables](https://github.com/btcpayserver/btcpayserver-docker#environment-variables) を設定して BTCPay を構成します。

```bash
export BTCPAY_HOST="btcpay.local"
export REVERSEPROXY_DEFAULT_HOST="$BTCPAY_HOST"
export NBITCOIN_NETWORK="mainnet"
export BTCPAYGEN_CRYPTO1="btc"
export BTCPAYGEN_LIGHTNING="clightning"
export BTCPAYGEN_REVERSEPROXY="nginx"
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="opt-more-memory"
export BTCPAY_ENABLE_SSH=true
```

複数のホスト名を使いたい場合は、任意の `BTCPAY_ADDITIONAL_HOSTS` 変数で追加します。

```bash
export BTCPAY_ADDITIONAL_HOSTS="btcpay.YourDomain.com"
```

ローカルネットワークのみにアクセスを制限したい場合は、`.local` ドメインを使う必要がある点に注意してください。

BTCPay のインストールを実行します。

```bash
. ./btcpay-setup.sh -i
```

数分以内に起動するはずです。ブラウザーで http://btcpay.local を開いてみてください。すべて正しく設定されていれば、BTCPay Server のトップページが表示されます。

あとは Bitcoin ブロックチェーンが [sync and full verify](../FAQ/Synchronization.md) されるまで、1日程度待つだけです。BTCPay Server の Web GUI 下部に進捗確認用のポップアップが表示されます。

### FastSync（任意）

[FastSync](/Docker/fastsync.md) が何か、また UTXO セットを自分で検証することがなぜ重要かを、注意深く読んで理解してください。

FastSync を利用すると、[悪意ある UTXO set snapshot](https://github.com/btcpayserver/btcpayserver-docker/blob/master/contrib/FastSync/README.md#what-are-the-downsides-of-fast-sync) を受け取った場合に攻撃へさらされます。
別の場所に信頼できるノードがある場合は、[these instructions](https://github.com/btcpayserver/btcpayserver-docker/blob/master/contrib/FastSync/README.md#dont-trust-verify) に従って FastSync で取得した UTXO セットの妥当性を確認できます。

```bash
# Stop BTCPay Server
cd /root/btcpayserver/btcpayserver-docker
./btcpay-down.sh

# Import FastSync UTXO set
cd contrib/FastSync
./load-utxo-set.sh
```

FastSync は高速回線で現在およそ 30 分かかります。
FastSync 完了後、次のコマンドで BTCPay Server を再起動します。

```bash
cd ../..
./btcpay-up.sh
```
