# ハードウェアデプロイ

Bitcoin を真に自己主権かつトラストレスに運用するには、**自分のハードウェア**とインターネット回線でノードを動かすことを検討してください。
**BTCPay Server は Bitcoin ノードと Lightning ノードの両方を動かす優れた方法です**。
トランザクションを検証できるだけでなく、ベースレイヤーの Bitcoin 支払いと第2レイヤーの Lightning 支払いを受け取れるようになります。
以下は、BTCPay Server を自分でインストールしてホストする手順です。

プロセスは基本的に次のとおりです。

1. ハードウェアを購入して組み立てる。
2. ベースとなる OS をインストールし、ネットワーク設定を行う。
3. BTCPayServer-Docker をインストールする。

**BTCPay Server は次のハードウェアでインストールできます**。最終的には、ブロックゼロから同期できる十分な性能を持つ、小型で静かなノードになります。総額はおおよそ $300 です。

1. [BeeLink S12 - Mini PC - $169.00](https://www.amazon.com/dp/B0C89TQ1YF?ref=nb_sb_ss_w_as-reorder-t1_k0_1_4&amp=&crid=SHKYOXZIRAO0&amp=&sprefix=beel)
3. [WD Blue 2TB SSD - $129.00](https://www.amazon.com/Western-Digital-SA510-Internal-Solid/dp/B0C14TF467/ref=sr_1_3?crid=2WDY52E7ESSEB&dib=eyJ2IjoiMSJ9.MBxkb5ZIvwjKXOzscB0GUvsbhX1rVhilXNFzID6n0xHORsDBPkIxQhIixVuiLY9I16rlFs5COExAAD8761Do-tzuAnZiutbqN-KM9rAL4zCw94kA_ArCJeR_RTDynZbiXf2Phnahw1Gw2dqXVek3p0dpe6_a_fbJrqx4BRaieoYo0zj1mX6YPGaYZAmF2Vf_Quk1TrkARk6s1_wZ0vFUw7EWdjKJ9hmNLxPWMfADML90A1rXk8gSCcRnwV2jdzN7jCfg2_urfJZ3IWOW5X3iwnP7s-vSec88PGmQ3RhS-Rc.sEURveFhiTAHYwZQdwyJX72hpWL5UgD_3tEPet747oE&dib_tag=se&keywords=2tb+ssd+wd+blue&qid=1710685725&s=electronics&sprefix=2tb+ssd+wd+blue%2Celectronics%2C90&sr=1-3)

その他に必要なもの:

1. 高速インターネット接続。
2. 固定 IP。
3. ドメイン名。
4. ルーターでポートを開放できること（任意。BTCPayServer は TOR や Dynamic DNS 経由でもアクセス可能）。
5. 小型ドライバー。
6. USB メモリ。
7. USB キーボード、マウス、モニター（初回インストール時のみ。完了後はヘッドレス運用可能）。

上記ハードウェアを購入した前提で、組み立て手順は次のとおりです。

### ドメイン名を設定する
DNS 変更の反映には数時間かかることがあるため、この手順を最初に行ってください。
ドメインレジストラにログインし、ドメインから自宅回線のグローバル IP アドレスに向けて A レコードを設定します。
サブドメイン（例: btcpay.yourdomain.com）の利用を推奨します。
グローバル IP アドレスは Google で "whats my ip" と検索すると確認できます。

### Lightning in a Box (LIAB) を組み立てる
- ドライバーで背面カバーを外す。
- SSD を挿入する。
- 付属ケージを使ってドライブを取り付ける。

### [Ubuntu 22.04 LTS Server](https://releases.ubuntu.com/jammy/ubuntu-22.04.4-live-server-amd64.iso) をダウンロードする

### [Balena Etcher](https://etcher.balena.io/) をダウンロードしてインストールする
Etcher は OS イメージを SD カードや USB ドライブへ書き込むソフトウェアです。
ここでは Etcher を使って、USB メモリに Ubuntu OS を書き込みます。

### USB キーボード、マウス、モニター、USB メモリを接続する
電源ボタンを押して LIAB を起動します。`DEL` キーを押して BIOS に入り、USB メモリを最優先で起動するようブート順を変更します。
Ubuntu のインストールは比較的シンプルで、手順に沿って進めやすいです。Ubuntu 公式チュートリアル: [Install Ubuntu Server](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview)。BeeLink S12 には Windows がプリインストールされているため、NVME パーティションを削除してそのドライブへ Ubuntu をインストールする必要があります。

*インストール中にホスト名を "btcpay" に設定し、SSH を有効化してください。

### ローカルネットワーク上で LIAB に固定 IP を割り当てる
方法はいくつかあり、オンラインに多くの解説があります。比較的シンプルな記事: [How to configure a static IP address on Ubuntu 22.04](https://www.linuxtechi.com/static-ip-address-on-ubuntu-server/)。ネットワーク内の他デバイスと競合しないよう、LIAB に対して IP "reservation" も設定してください。

### ルーターにログインし、ポート 80, 443, 9735 を LIAB のローカル IP に転送する（任意。.local や Tor のみを使う場合は不要）
ルーターごとに手順が異なるため、"Port Forward + ルーターのメーカー名と型番" で検索して確認してください。

### Fail2ban、GIT、Avahi-Daemon をインストールする
- [Fail2ban](https://github.com/fail2ban/fail2ban/wiki/How-to-install-fail2ban-packages) は、サーバーへの接続を試み悪意ある挙動を示す IP をブロックします。GIT は github.com 上のリポジトリをクローン・管理するために使います。
- [Avahi](https://avahi.org/) は、mDNS/DNS-SD プロトコル群を通じてローカルネットワーク上のサービス検出を容易にする仕組みです。
新しいターミナルを開いて次のコマンドを実行してください。

```bash
sudo apt update
sudo apt install -y fail2ban git avahi-daemon
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

次のポートはどこからでもアクセス可能にする必要があります（サブネットを指定しない場合のデフォルトは `any`）。

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

### Docker をインストールする
```Bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```

### ストレージを設定する
```bash
fdisk /dev/sda
# type 'p' to list existing partitions
# type 'd' to delete currently selected partitions
# type 'n' to create a new partition
# type 'w' to write the new partition table and exit fdisk
mkfs.ext4 /dev/sda1
mkdir /mnt/usb
UUID="$(sudo blkid -s UUID -o value /dev/sda1)"
echo "UUID=$UUID /mnt/usb ext4 defaults,noatime,nofail 0 0" | sudo tee -a /etc/fstab
mount -a
```

### Docker ボリューム用のマウントを作成する

```bash
rm -rf /var/lib/docker
mkdir -p /var/lib/docker
mount --bind /mnt/usb /var/lib/docker
echo "/mnt/docker /var/lib/docker none bind,nobootwait 0 2" >> /etc/fstab
systemctl restart docker
```

### BTCPay Server をセットアップする

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
export BTCPAY_ENABLE_SSH=true
```

複数ホスト名を使いたい場合は、任意の `BTCPAY_ADDITIONAL_HOSTS` 変数で追加します。

```bash
export BTCPAY_ADDITIONAL_HOSTS="btcpay.YourDomain.com"
```

ローカルネットワーク内のみにアクセスを制限したい場合は、`.local` ドメインを使う必要がある点に注意してください。

BTCPay のインストールを実行します。

```bash
. ./btcpay-setup.sh -i
```

数分で起動するはずです。Web ブラウザで http://btcpay.local を開いてみてください。正しく設定されていれば BTCPay Server のフロントページが表示されます。

次に、Bitcoin ブロックチェーンの [sync and full verify](../FAQ/Synchronization.md) が完了するまで1日程度待ちます。進捗は BTCPay Server の Web GUI 下部に表示されるポップアップで確認できます。

### FastSync（任意）

[FastSync](/Docker/fastsync.md) が何であり、UTXO セットを自分で検証することがなぜ重要なのかを十分理解するため、必ず注意深く読んでください。

FastSync を使うと、[malicious UTXO set snapshot](https://github.com/btcpayserver/btcpayserver-docker/blob/master/contrib/FastSync/README.md#what-are-the-downsides-of-fast-sync) が送られた場合に攻撃リスクにさらされます。
別の信頼できるノードを持っている場合は、[these instructions](https://github.com/btcpayserver/btcpayserver-docker/blob/master/contrib/FastSync/README.md#dont-trust-verify) に従って FastSync で取得した UTXO セットの妥当性を確認できます。

```bash
# Stop BTCPay Server
cd /root/btcpayserver/btcpayserver-docker
./btcpay-down.sh

# Import FastSync UTXO set
cd contrib/FastSync
./load-utxo-set.sh
```

FastSync は高速回線で現在およそ30分かかります。
FastSync 完了後、次のコマンドで BTCPay Server を再起動します。

```bash
cd ../..
./btcpay-up.sh
```
