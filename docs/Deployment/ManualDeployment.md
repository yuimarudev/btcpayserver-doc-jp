<!-- legacy-anchor-aliases -->
<span id="testnet-specific-deployments"></span>
<!-- /legacy-anchor-aliases -->

# 最小構成の手動セットアップ

:::danger

#### 本番利用には非推奨

OS と Bitcoin セキュリティに十分な専門知識と自信がない限り、手動インストールは本番利用に推奨されません。不安がある場合は Docker デプロイ、または他の [デプロイ方法](./README.md) を利用してください。

#### 技術的な読解力があり、問題を自力で解決できることが必須です。このデプロイ方法についてコミュニティから手厚いサポートは提供されません。

:::

手順の概要は次の通りです。

1. [Bitcoin Core](https://bitcoincore.org) をダウンロードして同期する
2. [NBXplorer](https://github.com/dgarage/NBxplorer) をクローンして実行する
3. [BTCPay Server](https://github.com/btcpayserver/btcpayserver) をクローンして実行する

詳細は以下の動画も参照してください。

[![BTCPay Server - Setup](https://img.youtube.com/vi/Xo_vApXTZBU/mqdefault.jpg)](https://www.youtube.com/watch?v=Xo_vApXTZBU)

## 警告: 本番利用には非推奨

**手動インストール** は本番環境には推奨されません。学習目的でのみ使用してください。

代わりに [docker deployment](https://github.com/btcpayserver/btcpayserver-docker) を使用してください。

docker deployment では、簡単な更新方式が提供され、すべての構成要素が技術的な知識なしでも正しく連携されます。HTTPS も自動で設定されます。

## 一般的な手動インストール

以下の手順は Ubuntu 18.04 で実施したものです。ご自身の環境に合わせて調整してください。

Testnet 向けのデプロイについては、Bitcoin、.NET Core、NBXplorer、BTCPayServer のインストール後に [Commands for Running in Testnet Mode](#testnet-specific-deployments) を参照してください。

### 1) Bitcoin Core 0.19.1 をインストール

```bash
BITCOIN_VERSION="0.19.1"
BITCOIN_URL="https://bitcoin.org/bin/bitcoin-core-0.19.1/bitcoin-0.19.1-x86_64-linux-gnu.tar.gz"
BITCOIN_SHA256="5fcac9416e486d4960e1a946145566350ca670f9aaba99de6542080851122e4c"

# install bitcoin binaries
cd /tmp
wget -O bitcoin.tar.gz "$BITCOIN_URL"
echo "$BITCOIN_SHA256 bitcoin.tar.gz" | sha256sum -c - && \
mkdir bin && \
sudo tar -xzvf bitcoin.tar.gz -C /usr/local/bin --strip-components=2 "bitcoin-$BITCOIN_VERSION/bin/bitcoin-cli" "bitcoin-$BITCOIN_VERSION/bin/bitcoind"
rm bitcoin.tar.gz
```

### 2) .NET 8.0 SDK をインストール

私の Ubuntu 20.04 での例です（他の OS は [these instructions](https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#2004-) または [here](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) を参照）。

```bash
# Add Microsoft package repository
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Install the SDK
sudo apt-get update
sudo apt-get install -y apt-transport-https
sudo apt-get update
sudo apt-get install -y dotnet-sdk-6.0

## Check
dotnet --version
```

### 3) NBXplorer をインストール

```bash
cd ~
git clone https://github.com/dgarage/NBXplorer
cd NBXplorer
git checkout latest
./build.sh
```

### 4) BTCPayServer をインストール

```bash
cd ~
git clone https://github.com/btcpayserver/btcpayserver
cd btcpayserver
git checkout latest
./build.sh
```

### 5) bitcoind を実行

```bash
bitcoind
```

### 6) NBXplorer を実行

```bash
cd ~/NBXplorer
./run.sh --dbtrie
```

`--dbtrie` バックエンドは簡単に使える一方で、NBXplorer では非推奨です。
[Extended Manual Deployment](./ManualDeploymentExtended.md) にある通り、postgresql バックエンドの利用を推奨します。

### 7) BTCPay Server を実行

```bash
cd ~/btcpayserver
./run.sh --port 8080 --bind 0.0.0.0
```

これでポート 8080 でサーバーにアクセスできます。

デフォルトでは BTCPay Server は SQLite をバックエンドとして使用しますが、これは簡単な反面非推奨です。
[Extended Manual Deployment](./ManualDeploymentExtended.md) にある通り、postgresql バックエンドの利用を推奨します。

## Testnet 向けデプロイ

Bitcoin、.NET Core、NBXplorer、BTCPayServer のインストールは上記手順に従ってください。

その後、実行時に次を使用します。

### bitcoind を testnet モードで実行

```bash
bitcoind -testnet
```

### NBXplorer を testnet モードで実行

```bash
cd ~/NBXplorer
./run.sh --network=testnet
```

### BTCPayServer を testnet モードで実行

```bash
cd ~/btcpayserver
./run.sh --port 8080 --bind 0.0.0.0 --network testnet
```

## 追加リンク

- [Extended Manual Deployment](./ManualDeploymentExtended.md)
- freedomnode.com の [How to Setup BTC and Lightning Payment Gateway with BTCPayServer on Linux [Manual Install]](https://freedomnode.com/blog/114/how-to-setup-btc-and-lightning-payment-gateway-with-btcpayserver-on-linux-manual-install)
