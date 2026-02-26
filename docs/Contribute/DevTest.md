# BTCPay Server のテスト

ソフトウェアのテストは、プロジェクトへ貢献する優れた方法です。ソフトウェアを _テスト_ する方法は数多くあります。ソフトウェアや機能を手動テスト（QA）し、ユーザー体験の観点でのフィードバックやバグ報告をプロジェクトの開発者・デザイナーへ提供してくれるユーザーは、常に歓迎されています。

オープンソースのソフトウェアなので、誰でもコードをテスト・監査できます。加盟店や技術的なユーザーの中には、新機能や既存機能を自分で検証したい人もいます。コードに取り組む開発者にとっても、BTCPay の特定の操作を手動でどうテストするかを理解することは有益です。

このガイドでは、一般的な BTCPay 機能を手動テストする方法を説明します。前提として、すでに [ローカル開発環境](./DevCode.md) がセットアップ済みであることを想定しています。基本的なテスト手順を理解すれば、ほかの機能も同様の方法で手動テストできます。

[[toc]]

## Regtest ネットワークとローカル BTCPay Server のセットアップ

まず、以下を完了していることを確認してください。

- オプション 1: 最新コードをテスト - [master を取り込む](./DevCode.md#sync-forked-btcpayserver-repository)
- オプション 2: 新機能をテスト - [Pull Request のブランチを作成する](./DevCode.md#create-a-branch-of-a-pull-request)
- ローカルの [Regtest Network](./DevCode.md#bitcoin-regtest-network-setup) を作成済み
- ソリューションをビルドし、[Browser mode](./DevCode.md#build-local-btcpayserver-in-browser-mode) または [Debug mode](./DevCode.md#build-local-btcpayserver-in-debug-mode) を起動済み

## ライブテストでローカル Docker イメージを使う

Docker Hub を使いたくない場合は、任意の open pull request や feature branch を使って、ローカル Docker イメージによるライブ環境テストが可能です。

ステップ 1:

BTCPay インスタンスにログインします:
```bash
ssh user@your-btcpay-server.tld
```

ステップ 2:

テストしたい BTCPay Server ブランチをクローンします。自分のブランチでも、他のコントリビューターのブランチでも構いません。`MYREMOTE` と `FEATUREBRANCH` は適宜置き換えてください:
```bash
# Clone the repository next to your btcpayserver-docker directory or somewhere else, does not really matter.
git clone git@github.com:MYREMOTE/btcpayserver.git btcpayserver-images
cd btcpayserver-images
# Checkout the branch you want to test
git checkout FEATUREBRANCH
```

ステップ 3:

現在デプロイ中のタグを確認し、同じタグで Docker イメージをローカルビルドします。

:::tip
以下の説明どおり、現在デプロイ中の BTCPay Server タグを使うと、docker-compose ファイル側でタグを変更する追加手順を省けます。ローカルビルドは常にリモートビルドを上書きするためです。もちろん、独自タグでビルドして docker compose ファイルをそのタグに変更し、BTCPay のセットアップを再実行する方法でも構いません。
:::

最初に、BTCPay Server インスタンスの現在タグを確認します:
```bash
docker ps | grep generated_btcpayserver | awk '{print $2}'
# output: btcpayserver/btcpayserver:1.13.1
```

次に、現在タグを上書きする形で Docker イメージをビルドします。ここでは `btcpayserver/btcpayserver:1.13.1` です:
```bash
docker build -t btcpayserver/btcpayserver:1.13.1 .
```

ステップ 4:

`btcpayserver-docker` ディレクトリへ移動し、`btcpay-up.sh` を実行します:
```bash
cd $BTCPAY_BASE_DIRECTORY/btcpayserver-docker
./btcpay-up.sh
```

これで、独自の BTCPay Server イメージで動作する状態になります。

## Mainnet テストで Docker イメージを使う

一部の機能は localhost の開発環境では十分にテストできません。統合系の機能では、mainnet や testnet での実際の支払いが必要になることがあります。ここでは、未リリース機能を含むカスタム Docker イメージをライブサーバーへデプロイしてテストする方法を説明します。

ステップ 1:

[BTCPay Server リポジトリ](https://github.com/btcpayserver/btcpayserver) を [fork・clone してブランチを作成](./DevCode.md#git-setup) し、ブランチ名を `btcpay-branch` にします。新しいブランチで変更を加えます（例: [この行](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer/Views/UIHome/Home.cshtml#L31) を変更する）。

ステップ 2:

[BTCPay Server Docker リポジトリ](https://github.com/btcpayserver/btcpayserver-docker) を [fork・clone してブランチを作成](./DevCode.md#git-setup) し、ブランチ名を `docker-branch` にします。

ステップ 3:

Docker Hub アカウントを作成し、Docker リポジトリを用意し、Docker Desktop をダウンロードしたうえで、[この手順](https://docs.docker.com/docker-hub/) に従ってログインします。

ステップ 4:

BTCPay Server はブロックチェーン同期を必要とするため、すでにデプロイ済みかつ同期済みのサーバーを使うのが簡単です。このサーバーは、ステップ 2 で作成した自分の `docker-branch` を参照してデプロイする必要があります。以下は [LunaNode launcher](https://launchbtcpay.lunanode.com/) を使った例です。

![LunaNode Fork](../img/Contribute/lunanode-fork.png)

:::warning
上の画像が示すとおり、ステップ 2 で作成した fork & clone 済み btcpayserver-docker リポジトリの GitHub URL とブランチ名を指定する必要があります。
:::

ステップ 5:

`btcpay-branch` のルートディレクトリには、`amd64`、`arm32v7`、`arm64v8` という接頭辞の Dockerfile があります。利用 OS に対応する Dockerfile を使って、カスタムイメージを build および push します。

`<dockerUser>` は Dockerhub ユーザー名に置き換えてください。タグ `1.13.1` は独自のバージョンタグに置き換えるか、以下のコマンドでは `latest` タグを使用してください。

```docker
#build image
docker build -t <dockerUser>/btcpayserver:1.13.1 --file ./amd64.Dockerfile .

#push image
docker push <dockerUser>/btcpayserver:1.13.1
```

ステップ 6:

Docker Hub リポジトリにイメージが表示され、タグがステップ 5 で指定したものと一致していることを確認します。

ステップ 7:

ステップ 2 で作成したローカル `docker-branch` にある [btcpayserver.yml docker-fragment](https://github.com/btcpayserver/btcpayserver-docker/tree/master/docker-compose-generator/docker-fragments) を見つけます。btcpayserver イメージの参照リポジトリを、自分の Docker イメージへ置き換えます。`<dockerUser>` は Dockerhub ユーザー名に、タグバージョン（例: `1.13.1`）はステップ 5 で使ったものに置き換えてください。

```yaml
image: ${BTCPAY_IMAGE:-<dockerUser>/btcpayserver:1.0.0.1$<BTCPAY_BUILD_CONFIGURATION>?}
```

ステップ 8:

ローカル `docker-branch` の変更を、GitHub 上の BTCPayServer Docker リポジトリへ push します。

ステップ 9:

[サーバーを更新する](../FAQ/ServerSettings.md#how-to-update-btcpay-server)。

これで、機能がすでにリリース済みであるかのようにテストできます。

## 請求書を作成する

請求書の作成と支払い送信は BTCPay の重要機能です。これを手動テストするには、まず次を行ってください。

- Store を作成する
- Wallet をセットアップする

:::tip
テスト時に最速で Wallet を用意するには hot wallet を使ってください。Import from ... > a new/existing seed > check Is hot wallet > Generate
:::

- Store 用の請求書を作成する

<a id="pay-invoice"></a>
## 請求書を支払う

新しい Powershell ターミナルを開き、プロジェクトの Docker-Compose コマンドを実行している `BTCPayServer.Tests` ディレクトリに移動します。請求書から支払い金額とアドレスをコピーし、次のコマンドへ反映します。

`.\\docker-bitcoin-cli.ps1 sendtoaddress "bcrt1qym96l8gztggldraywdumgmfw27u8p8h5w7h9kc" 0.00097449` を実行して `Enter` を押します。

ローカル BTCPay Server で請求書が支払い済みになったことを確認してください。

![Test Paid Invoice](../img/Contribute/regtest-paid-invoice.png)

他の支払いタイプのテストについては、[このガイド](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer.Tests/README.md) を参照してください。

## テスター FAQ

### デバッグ開始時に次のエラーが出る: No connection could be made because the target machine actively refused it. 127.0.0.1:39372

このエラーが表示される場合、`BTCPayServer.Tests` ディレクトリで `docker-compose up dev` コマンドを使った Regtest Network のセットアップが未実施であることを意味します。このコマンドは、ローカル開発環境で BTCPay が利用するサービスの依存関係をセットアップします。デバッグ開始前に必ず実行してください。

### Regtest の支払いが確認済みにならない場合

[テスト支払い](#pay-invoice) を実行して未確認状態のままなら、トランザクションに承認を付けるためにブロックを採掘してください。

```powershell
.\docker-bitcoin-generate.ps1 3
```

テスト支払い通知など、期待するイベントが欠けている場合も、これが原因の可能性があります。

### メジャーリリースではどのブランチをテストすべきですか？

master ブランチのテストは許容されます。リリース変更が含まれるためです。ただし、未リリースのコミットが master に含まれる場合もあります。リリース前に問題を見つけることは重要なので、master（または特定の PR）は理想的なテスト対象ブランチです。

現在のデプロイで利用可能な変更と未リリースコミットを確認するには、[最新リリース](https://github.com/btcpayserver/btcpayserver/releases) を参照してください。

### 請求書を支払い済みにマークできますか？

いいえ、請求書を手動で支払い済みにマークすることはできません。開発目的で完了済み支払い状態が必要な場合は、[請求書を支払う](#pay-invoice) を実行するか、`$0` の請求書を作成してください（作成時に自動で支払い済みになります）。
