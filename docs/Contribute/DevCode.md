# 開発環境のセットアップ

[[toc]]

このガイドでは、BTCPay Server リポジトリへの今後の貢献に備えて、開発環境をセットアップする方法を説明します。開発を始めやすくするため、以下では初心者向けのさまざまなツールを使います。全体的なセットアップ手順を理解したら、使いやすいツールに置き換えて構いません。

タイポ修正や文言修正のような簡単なコード変更方法を探している場合は、簡略版チュートリアルの [Write Software](./WriteSoftware.md) を参照してください。すでにローカル開発環境を用意している上級開発者は、[LocalDevelopment](/Development/LocalDevelopment.md) のドキュメントに進んでください。

## 開発者向けリソース

- [GitHub ドキュメント](https://docs.github.com/)
- [BTCPay のコマンドと概念](/Development/LocalDevelopment.md)
- [環境セットアップ動画（Linux, Mac, Windows）](/Development/LocalDevelopment.md#videos)

## Windows セットアップ用ソフトウェア

このガイドを進めるために、以下のソフトウェアをインストールしてください。

1.  [Visual Studio Community Edition](https://visualstudio.microsoft.com/downloads/)
1.  [.NET Core SDK 8.0+](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
1.  [Docker Desktop](https://www.docker.com/products/docker-desktop)
1.  PowerShell（Windows OS に含まれています）
1.  [GitBash](https://gitforwindows.org/)
1.  [GitHub Desktop](https://desktop.github.com/)
1.  [www.Github.com アカウント](https://github.com/)（サインアップ）

注: _このガイドは、各ソフトウェアを既定の場所にインストールしている前提です。フォルダ構成が異なる場合は、適宜読み替えてください。_

<a id="git-setup"></a>
## Git セットアップ

### BTCPay Server リポジトリをフォークする

- Web ブラウザを開き、www.Github.com のアカウントにログインします。
- [BTCPay Server Repository](https://github.com/btcpayserver/btcpayserver) に移動し、`Fork` ボタンを押して自分用の BTCPay Server リポジトリを GitHub 上に作成します。
- 続いて GitHub Desktop を開いてログインし、GitHub Desktop が www.Github.com アカウントと接続できるようにします。

### BTCPay Server リポジトリをクローンする

- GitHub Desktop で `Add` ボタンを使い、リポジトリをクローンするオプションを選びます。
- GitHub Desktop で www.Github.com の認証情報を使用している場合、先ほど www.Github.com でフォークした BTCPay Server リポジトリが表示されます。それを選び、下に表示されるローカルパスを確認してください。（既定では `C:\Users\SatoshisComputer\Documents\GitHub\btcpayserver` のようなパスになります。以降これを _clone local path_ と呼びます）その後クローンを実行します。
- GitHub Desktop 上で BTCPay Server リポジトリのクローンが完了し、_master ブランチ_ 上にいる状態になります。

### 開発用フィーチャーブランチを作成する

- 次に、GitHub Desktop でコンピュータにクローンした BTCPay Server リポジトリを使う練習をします。
- 開発では、複数の機能を同時に進めることがあります。その場合、すべての変更を master ブランチに入れるのではなく、複数のフィーチャーブランチを作るのが一般的です。
- ここでは GitBash といくつかの Git コマンドを使うので、GitBash を開きます。（GitBash を使わず GitHub Desktop だけで進めたい場合は、そちらでブランチを作成しても構いません）
- GitBash ターミナルを開いたら、BTCPay Server リポジトリのクローン先ディレクトリに移動する必要があります。
- これを行うには、`cd` コマンドで _clone local path_ に移動します: `$ cd Documents/Github/btcpayserver`
- BTCPay Server のクローンが `master` ブランチにあることを確認できます。
- master ブランチを開発用に複製するため、次のコマンドを実行します: `$ git branch OurNewDevelopmentBranch`
- 現在のブランチ一覧を確認します: `$ git branch`。`master` と `OurNewDevelopmentBranch` が表示されます。
- Git では、フォークした BTCPay Server リポジトリ（クローン）のコピーを扱っています。ブランチ（クローンのコピー）を切り替えるときは、どのブランチに開発中の変更を紐づけるかを Git に伝える必要があります。次のコマンドでブランチを checkout します: `$ git checkout OurNewDevelopmentBranch`
- これで GitBash で `OurNewDevelopmentBranch` に切り替わりました。
- GitHub Desktop を開くと、master ではなく `OurNewDevelopmentBranch` にいることを確認できます。
- GitHub Desktop の上部メニューで `Repository > Show In Explorer` をクリックすると、ファイルの場所を確認できます。

## ローカル BTCPay セットアップ

<a id="bitcoin-regtest-network-setup"></a>
### Bitcoin Regtest ネットワークのセットアップ

- 次の手順に進む前に、Docker-Compose がインストールされていることを確認してください（Docker Desktop に含まれます）。PowerShell ターミナルを開き、_clone local path_ から BTCPayServer.Tests ディレクトリへ移動します: `$ cd Documents/Github/btcpayserver/BTCPayServer.Tests`
- BTCPay Server.Tests プロジェクトには、プロジェクト依存サービスを起動し、ローカル Regtest ネットワークを作成するための Docker ファイルが含まれています。
- PowerShell で、次のコマンドを実行して Docker サービスを起動します: `docker-compose up dev`（このコマンドは BTCPay Server.Tests ディレクトリ内で実行する必要があります）。
- PowerShell ターミナルには、最初に必要な Docker イメージの取得（pull）、その後コンテナのビルドが表示されます。ビルドが成功すると、すべてのコンテナが完了状態になります。

![BTCPayServer.Tests の PowerShell ターミナル](../img/Contribute/docker-compose-up-dev.png)

<a id="build-local-btcpayserver-in-browser-mode"></a>
### ブラウザーモードでローカル BTCPay Server をビルドする

コーディングは行わず、インターフェース機能の検証用にローカル BTCPay Server だけを作成したい場合は、コマンドラインから起動できます。

[Regtest ネットワーク](#bitcoin-regtest-network-setup) の構築後、`btcpayserver\BTCPayServer` ディレクトリへ移動して次のコマンドを実行します。

```bash
dotnet run --launch-profile Bitcoin
```

新しいブラウザを開き、[http://127.0.0.1:14142](http://127.0.0.1:14142) にアクセスします。

### Visual Studio のセットアップ

- ファイルエクスプローラーで BTCPay Server リポジトリのフォルダを開きます。表示されているフォルダは開かずに、`btcpayserver.sln` を探して右クリックし、`Open with > Visual Studio` を選択します。初めてこの種類のファイルを開く場合は、`Open with > Choose another app ...` から Visual Studio を選ぶ必要があるかもしれません。
- Visual Studio をセットアップするには、上部メニューで `View > Solution Explorer` を選びます。この Solution Explorer で BTCPay Server のファイルとフォルダを確認できます。
- 最上位プロジェクトは BTCPay Server です。太字表示になっていることを確認してください。なっていない場合は右クリックして Set as StartUp Project を選択します。
- これで Visual Studio のセットアップは完了です。

![Visual Studio の Solution Explorer](../img/Contribute/vs-solution-explorer.png)

<a id="build-local-btcpayserver-in-debug-mode"></a>
### デバッグモードでローカル BTCPay Server をビルドする

- Visual Studio に戻り、`Build > Build Solution` をクリックします。
- Output ウィンドウでは、成功時に次のような表示になります: `========== Build: 6 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========`
- 次に `Debug > Start Debugging` を押します。
- まず Visual Studio のデバッグコンソールが開き、ローカル BTCPay Server の状態情報が表示されます。
- 続いて Web ブラウザでローカル BTCPay Server が起動し、ホーム画面で `REGTEST` モードになっていることが表示されます。
- この時点で、次の3つの画面を確認できます: BTCPay Server のブラウザセッション、Visual Studio のデバッグコンソール、BTCPay Server.Tests の PowerShell ターミナル。
- BTCPay Server で新規ユーザーを登録し、その登録イベントが Visual Studio のデバッグコンソールに表示されることを確認します。

![Visual Studio のデバッグコンソール](../img/Contribute/vs-debug-console.png)

![ローカル Regtest の BTCPay](../img/Contribute/dashboard-change.jpg)

### ローカル BTCPay Server で Visual Studio からコード変更を確認する

- Visual Studio でコードを変更します（例: `~\BTCPayServer\Views\UIStores\Dashboard.cshtml` の `This store is ready to accept transactions, good job!` というテキストを変更する）
- ページを更新し、ホーム画面にテキスト変更が反映されることを確認します。
- コード変更によっては、反映のために Debugging の再起動が必要です。
- Visual Studio でブレークポイントを設定し、ローカル BTCPay Server の機能を操作したときにそのブレークポイントで停止することを確認します。

## Git メンテナンス

<a id="sync-forked-btcpayserver-repository"></a>
<a id="sync-forked-btcpay-server-repository"></a>
### フォークした BTCPay Server リポジトリを同期する

- 多くのコントリビューターが BTCPay Server のメインリポジトリにコードを追加するため、新しい変更をフォークへ取り込まないと、自分のフォークが遅れることがあります。
- www.Github.com 上の自分の BTCPay Server フォークを見ると、ブランチが何コミット遅れているかを示すメッセージが表示されます。例: `This branch is 32 commits behind btcpayserver:master`。
- 更新には GitBash を使うか、GitHub Desktop の同期プロンプトをたどる方法があります。
- GitBash ターミナルを開き、以下のコマンドで BTCPay Server リポジトリを更新します。
- まず _clone local path_ に移動し、`master` ブランチにいることを確認します: `$ cd Documents/Github/btcpayserver`。

```bash
$ git fetch upstream
$ git merge upstream/master
$ git commit -m <SomeCommitMessage>

メッセージ例: ...your branch is ahead of origin master by "X" commits... use git push to publish...

$ git add .
$ git push origin master
```

`$ git fetch upstream` 実行時に `fatal: 'upstream' does not appear to be a git repository` というエラーが出る場合は、先に upstream リポジトリを指す remote を Git に設定する必要があります。これは最初の1回だけ必要です。_clone local path_ にいる状態で次のコマンドを実行してください。

```bash
$ git remote add upstream https://github.com/btcpayserver/btcpayserver.git

# upstream リポジトリが正しく追加されたか確認
$ git remote -v

# 次のような表示になります:
origin	YOUR_FORKED_GITHUB_REPO (fetch)
origin	YOUR_FORKED_GITHUB_REPO (push)
upstream	https://github.com/btcpayserver/btcpayserver.git (fetch)
upstream	https://github.com/btcpayserver/btcpayserver.git (push)
```

### プルリクエスト作成のためにコードをコミットする

- フィーチャーブランチ（例: `Fix/BugBranch`）でコード変更を行い、BTCPay Server Repository に Pull Request を作成したい場合は、GitBash ターミナルを開いて _clone local path_ へ移動します: `$ cd Documents/Github/btcpayserver`。その後、コミット対象の **正しいブランチ** にいることを確認し、`git status` で変更ファイルが意図どおりか確認します。

```bash
$ git status
$ git add .
$ git commit

テキストエディターが開き、コミットメッセージを入力できます...
コミットメッセージ例: update button のバグを修正

変更を確定: Ctrl + x
保存: Shift + y
Enter でエディターを閉じる

$ git push origin Fix/BugBranch
```

www.Github.com 上の BTCPay Server Fork に新しいブランチが作成されたことを確認し、変更内容をレビューして Pull Request を作成します。

<a id="create-a-branch-of-a-pull-request"></a>
### プルリクエストのブランチを作成する

高度な開発者でなくても貢献しやすい方法として、他のコントリビューターの Pull Request をテストすることがあります。手動テストは、他の人の変更を検証し、BTCPay Server のコード変更が正しく動作することを確認するのに有効です。ここでは、以前の PoS Pull Request https://github.com/btcpayserver/btcpayserver/pull/454 を例に、他者の Pull Request からブランチを作る方法を示します。GitBash ターミナルを開き、_clone local path_ に移動します: `$ cd Documents/Github/btcpayserver`。`git status` でステージ済みコミットがない（git status が clean）ことを確認してください。

```bash
$ git status
$ git fetch upstream pull/454/head:pos-new-design
$ git branch（新しいテスト用ブランチ pos-new-design が表示されます）
```

注: テストしたい pull request 番号に合わせて `/454/` を変更してください。通常、`/head:` はそのままで問題なく、その後ろに pull request のブランチ名を指定します。

### ローカルブランチを削除する

Github.com のフォーク済み BTCPay リポジトリでブランチを削除しても、ローカルマシン上のコピーは削除されません。必要なら次のように削除します。

```bash
$ git checkout master
$ git branch -D <branch name>
```

注: checkout 中のブランチは削除できないため、上記の例のように先に `master` など別ブランチへ切り替えてください。

## Docker コンテナを扱う

ローカル開発中に Docker コマンドを使う場合は、`BTCPayServer.Tests` ディレクトリで次のコマンドを実行できます。

- 実行中コンテナを表示 `docker ps`
- コンテナのログを表示 `docker ps logs <container>`
- Docker コンテナを起動 `docker-compose up dev`
- Docker コンテナを停止 `docker-compose down`
- Docker コンテナを破棄 `docker-compoose down --v`

## Greenfield API 開発

BTCPay Greenfield API は [現在も開発中](../FAQ/General.md#how-can-i-use-the-btcpay-server-api) です。利用例は [こちらの例](../Development/GreenFieldExample.md) で確認できます。BTCPay REST API を使って開発したい開発者向けに、公式の Greenfield [API リファレンスドキュメント](https://docs.btcpayserver.org/API/Greenfield/v1/) も用意されています。

Greenfield API に貢献したい開発者は、追加や修正時に BTCPay プロジェクトの [開発者ガイドライン](https://github.com/btcpayserver/btcpayserver/blob/master/docs/greenfield-development.md) に従ってください。ガイドラインが不明瞭だと感じる場合は、コミュニティチャット（development channel）で相談するか、エンドポイント実装案を議論するために [GitHub issue を作成](https://github.com/btcpayserver/btcpayserver/issues/new/choose) することを検討してください。

## データベースを扱う

BTCPay は既定で PostgreSQL データベースを使用します。開発中は簡単に接続でき、データ保存の確認、レコード修正、開発時の問題調査に役立ちます。これには無料ツールの [PgAdmin4](https://www.pgadmin.org/download/) を利用できます。

ローカル環境で BTCPay を起動し、デバッグコンソールでデータベース接続情報を確認します。

![PostgreSQL 設定](../img/Contribute/DB-Config.png)

次に PgAdmin を開き、`Servers > Create > Server...` を選んでサーバーに接続します。サーバー名を設定し、Visual Studio のデバッグコンソールに表示されたホスト接続情報を入力します。

![PgAdmin 接続](../img/Contribute/DB-Connect.png)

保存して開発用 btcpayserver データベースに接続します。btcpayserver データベースでは、
`Schemas > public > Tables` を確認すると BTCPay Server データを保持しているテーブルを参照できます。

例として、`AspNetUsers` テーブルの行を表示すると、開発用 BTCPay に登録されているユーザーをすべて確認できます。データベース内で登録済みユーザー名を変更し、`Save Changes` と `Refresh (F5)` を実行してみてください。その後、新しいユーザー名と元のパスワードで BTCPay にログインできます。

![PgAdmin での編集](../img/Contribute/DB-Edit.png)

## 質問

BTCPay Server のローカル開発セットアップについて質問がある場合は、[コミュニティチャット](https://chat.btcpayserver.org/) に参加できます。その他のツールやコマンドについての質問は、インターネット検索や [StackOverflow](https://stackoverflow.com/) でも解決策を見つけられることが多いです。
