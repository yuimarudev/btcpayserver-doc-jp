# ローカル開発

## 前提条件

**開発環境** では次のツールをインストールする必要があります。

- [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
- Docker: [Windows](https://docs.docker.com/docker-for-windows/install/) | [Mac OS](https://docs.docker.com/docker-for-mac/install/) | [Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## 依存関係

テスト実行やデバッグ実行のために、いくつかの **依存サービス** を起動する必要があります。

依存関係はすべて docker-compose ファイルにまとめてあり、これを使って開発環境を立ち上げられます。
[BTCPayServer.Tests/docker-compose.yml](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer.Tests/docker-compose.yml) を使うと一式を起動できます。

```bash
git clone https://github.com/btcpayserver/btcpayserver.git
cd btcpayserver/BTCPayServer.Tests
docker-compose up dev
```

## どの IDE を使うべきか？

Visual Studio 2022（Windows のみ）または Rider（クロスプラットフォーム）を推奨します。Visual Studio Code（クロスプラットフォーム）でも可能ですが、快適な開発環境の構築はやや複雑です。
もちろん VIM も使えます。.NET Core はコマンドラインでも扱いやすいです。

Visual Studio Code、Visual Studio、Rider は launch profile `Bitcoin` を実行します。
これにより **Docker サービス内の依存関係へ接続する BTCPay Server インスタンス** が起動し、デバッグやステップ実行が簡単になります。

## ビルド構成

build configuration は **BTCPay Server のビルド方法** を定義します。たとえば、特定のソースを含めるかどうか、デバッグ最適化か性能最適化かなどです。

利用可能な build configuration は次のとおりです。

- `Debug`
- `Release`
- `Altcoins-Debug`
- `Altcoins-Release`

ローカル開発中に別構成を使う方法は IDE によって異なります。
デフォルトは `Debug` で、これは altcoin 依存関係を含まない Bitcoin 専用ビルドです。別構成の使い方は IDE に依存します。

`dotnet` コマンドラインでは `-c` スイッチでビルド構成を選択できます。コマンドラインで Release ビルドを実行する場合は次のとおりです。

```bash
dotnet run -c Release
```

## Launch profiles

**ローカル開発のために BTCPay Server をローカル起動** する際は、docker-compose の開発用依存関係へ接続するための適切なパラメータが必要です。

これらのパラメータは dotnet の `launch profile` という仕組みにまとめられています。

launch profile は [launchSettings.json](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer/Properties/launchSettings.json) で定義されています。

現在は次の3つがあります。

- `Bitcoin`
- `Bitcoin-HTTPS`
- `Altcoins-HTTPS`

デフォルトは `Bitcoin` です。別の profile を使う方法は IDE によって異なります。

コマンドラインを使う場合、`dotnet run` で任意の launch profile を選択できます。

```bash
dotnet run --launch-profile Bitcoin
```

## テスト実行

テスト実行は、開発時の BTCPay Server 実行とほぼ同じ仕組みで動作します。

```bash
cd BTCPayServer.Tests
dotnet test
```

テストでは `launch profile` の概念は適用されませんが、build configuration は適用されます。たとえば Release ビルドでテストしたい場合は次のとおりです。

```bash
dotnet test -c Release
```

テストは、先ほど示した docker-compose の開発用依存関係を使うよう、すでに設定済みです。

特定のテストだけを実行するには `--f`（filter）スイッチを使えます。

IDE を使う場合は、テスト実行や構成切り替えについて IDE のドキュメントを参照してください。

## Altcoin サポート開発

デフォルトでは、IDE または単純な `dotnet run` は `Debug` ビルド上の `Bitcoin` launch profile を使用します。

- これは、BTCPay Server がローカル HTTP ポートで起動し、altcoin サポートなしでビルドされることを意味します。
- また、[BTCPayServer.Tests/docker-compose.yml](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer.Tests/docker-compose.yml) に定義された Bitcoin 専用依存関係へ接続します。

**altcoins サポート付きで開発** したい場合は、`Altcoins-Debug` ビルドで `Altcoins-HTTPS` launch profile を使い、[BTCPayServer.Tests/docker-compose.altcoins.yml](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer.Tests/docker-compose.altcoins.yml) を実行してください。

コマンドラインを使う場合:

```bash
cd BTCPayServer.Tests
docker-compose -f docker-compose.altcoins.yml up dev
cd ../BTCPayServer
dotnet run -c Altcoins-Debug --launch-profile Altcoins-HTTPS
```

テストの場合:

```bash
cd BTCPayServer.Tests
dotnet test -c Altcoins-Debug
```

## ローカル開発での HTTPS サポート

一部ブラウザのセキュリティ機能を正しく検証するには **HTTPS** が必要な場合があります。

その場合は `Bitcoin-HTTPS`（または `Altcoin-HTTPS`）launch profile を使用します。開発用の自己署名証明書が作成されます。

ただし、ブラウザはこの証明書を信頼しないため、デバッグしづらくなることがあります。

次を実行すると、OS にこの開発用証明書を信頼させることができます。

```bash
dotnet dev-certs https --trust
```

## 動画

詳細は次の動画を参照してください。

- [BTCPay Server 開発への貢献方法（Windows）](https://youtube.com/watch?v=ZePbMPSIvHM) by Nicolas Dorier
- [Linux（Ubuntu）で BTCPayServer 開発環境を構築する](https://youtube.com/watch?v=j486T_Rk-yw) by RockStarDev
- [BTCPay Server 開発 - Pull Request と支払いのテスト（MacOS）](https://youtube.com/watch?v=GWR_CcMsEV0) by Pavlenex

あわせて次のノートも参照してください。

- [開発の始め方](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer.Tests/README.md) by Nicolas Dorier（関連する Docker コマンドと regtest 請求書の支払い手順を解説）
