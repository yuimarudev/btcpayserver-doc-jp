<!-- legacy-anchor-aliases -->
<span id="files"></span>
<span id="forgot-btcpay-admin-password"></span>
<span id="how-can-i-check-my-btcpay-server-version-via-terminal"></span>
<span id="how-can-i-see-my-btcpay-version"></span>
<span id="how-to-add-a-new-user-by-invite"></span>
<span id="how-to-add-google-analytics-code-to-btcpay"></span>
<span id="how-to-allow-registration-on-my-btcpay-server"></span>
<span id="how-to-configure-smtp-settings-in-btcpay"></span>
<span id="how-to-customize-my-btcpay-theme-style"></span>
<span id="how-to-disable-u2f-and-2fa-for-a-user"></span>
<span id="how-to-hide-my-btcpay-server-from-search-engines"></span>
<span id="how-to-modify-the-checkout-page"></span>
<span id="how-to-remotely-connect-to-my-btcpay-full-node"></span>
<span id="how-to-restart-btcpay-server"></span>
<span id="how-to-ssh-into-my-btcpay-running-on-vps"></span>
<span id="how-to-update-btcpay-server"></span>
<span id="how-to-upload-files-to-btcpay"></span>
<span id="maintainance"></span>
<span id="policies"></span>
<span id="services"></span>
<span id="theme-customization"></span>
<span id="what-is-btcpay-ssh-key-file"></span>
<span id="error-maintenance-feature-requires-access-to-SSH-properly-configured-in-btcpayserver-configuration"></span>
<!-- /legacy-anchor-aliases -->

# サーバー設定 FAQ

このドキュメントでは、サーバー設定に関する質問と問題を扱います。
これらの設定はサーバー管理者のみが利用できます。詳細は[ウォークスルーページ](../Walkthrough.md)を参照してください。

[[toc]]

## メンテナンス

### BTCPay Server を更新するには？

BTCPay Server を更新する方法は2つあります。

1. ユーザーインターフェースから更新: Server Settings > Maintenance > Update

![Updating BTCPay Server](../img/HowToUpdateBTCPayServer.png)

2. SSH で更新: `ssh` で仮想マシンにログインし、次のコマンドを実行します。

```bash
sudo su -
cd btcpayserver-docker
btcpay-update.sh
```

### BTCPay Server を再起動するには？

BTCPay Server を再起動する方法は2つあります。

1. ユーザーインターフェースから再起動: Server Settings > Maintenance > Restart

![Restarting BTCPay Server](../img/HowToRestartBTCPayServer.png)

2. SSH で再起動: `ssh` で仮想マシンにログインし、次のコマンドを実行します。

```bash
sudo su -
cd btcpayserver-docker
btcpay-restart.sh
```

### VPS 上で動作する BTCPay に SSH 接続するには？

ドメインまたは IP で SSH 接続するには、次を実行します。

```
ssh domainuser@example.com (domain)
or
ssh domainuser@70.32.86.175 (IP)

domainuser@example.com's password:
yourPassword
```

Enter キーを押します。

このコンピューターからサーバーへ初回接続する場合、次の出力が表示されます。

```
The authenticity of host 'example.com (70.32.86.175)' can't be established.
RSA key fingerprint is 3c:6d:5c:99:5d:b5:c6:25:5a:d3:78:8e:d2:f5:7a:01.
Are you sure you want to continue connecting (yes/no)?

yes
```

または、PuTTY を使ったこの [LunaNode example](https://github.com/JeffVandrewJr/patron/blob/master/SSH.md) を参照してください。

### 管理者として、BTCPay Server 上で何が動いているか確認するには？

BTCPay Server に SSH 接続し、次の1行でシステム内の `apps` 一覧を確認できます。

```
docker exec -ti $(docker ps -a -q -f "name=postgres_1") psql -U postgres -d btcpayservermainnet -c 'select "Name" from "Apps";'
```

`stores` とその Web サイトの一覧を確認するには次を実行します。

```
docker exec -ti $(docker ps -a -q -f "name=postgres_1") psql -U postgres -d btcpayservermainnet -c 'select "StoreName","StoreWebsite" from "Stores";'
```

ユーザー一覧を表示するには次を実行します。

```
docker exec -ti $(docker ps -a -q -f "name=postgres_1") psql -U postgres -d btcpayservermainnet -c 'select "Id", "Email" from "AspNetUsers";'
```

### BTCPay Server のバージョンを確認するには？

サーバー管理者としてログインしているとき、**ページフッター右下**で BTCPay Server のバージョンを確認できます。

v1.0.5.7 以降のデプロイでは、新しい BTCPay Server バージョンがリリースされると通知が自動表示されます。

![Version](../img/notifications/notification-version.png)

注: この機能は BTCPay Server コンテナー内で `BTCPAY_UPDATEURL` 環境変数を自動設定し、1日1回 [this Github endpoint](https://api.github.com/repos/btcpayserver/btcpayserver/releases/latest) にリクエストします。サーバー管理者は Server Settings > Policies > Check releases on GitHub のポリシーを無効化することで、この通知を無効にできます。

### ターミナルで BTCPay Server のバージョンを確認するには？

`btcpayserver-docker` フォルダーで次を実行します: `bitcoin-cli.sh getnetworkinfo`

### BTCPay SSH key file とは？

BTCPay SSH key を使うと、ユーザーインターフェースからサーバー更新やドメイン名変更を素早く行えます。

### BTCPay 管理者パスワードを忘れた場合は？

まず、BTCPay Server で「Register」をクリックして新規ユーザーを登録します。例: `newadmin@example.com`

Server Settings > Policies で登録が無効化されていて新規ユーザーを作成できない場合は、ポリシー設定をリセットする必要があります。フロントページの Register ボタンでユーザー作成できる場合、この手順は不要です。次のコマンドを実行してください（現在使われている他のサーバー設定も削除されます）。

```bash
# In root
sudo su -
cd $BTCPAY_BASE_DIRECTORY/btcpayserver-docker/
# Re-open registrations
./btcpay-admin.sh reset-server-policy
```

BTCPay Server に戻り、有効化された「Register」ボタンをクリックします。メニューに Register リンクが出ない場合はキャッシュが原因の可能性があります。`btcpay-restart.sh` で再起動してください。

次に、新規登録したユーザー `newadmin@example.com` を管理者に追加します。

```bash
# Set new user as admin
./btcpay-admin.sh set-user-admin newadmin@example.com
```

これで `newadmin@example.com` を管理者として利用できます。

管理者化する前にそのユーザーでログイン済みだった場合は、一度ログアウトして再ログインが必要です。

変更適用後、新しく作成した管理者ユーザーは自動的にはどのストアにも追加されません。BTCPayServer 2.0 以降なら新しいサーバー管理者を手動でストアに追加できます。古いバージョンでは、SMTP を設定してパスワードリセットメールを送るか、[R0ckstar Dev の **Admin Pass Reset** plugin](https://github.com/rockstardev/BTCPayServerPlugins.RockstarDev/tree/master/Plugins/BTCPayServer.RockstarDev.Plugins.AdminPassReset) を使うことで旧サーバー管理者アカウント（およびストア）へのアクセスを復旧できます。

### 招待で新規ユーザーを追加するには？

サーバー管理者は共有用の招待リンクを作成して新規ユーザーを追加できます。これによりサーバーで公開登録を無効化したまま、特定ユーザーのみを招待できます。操作は Server Settings > Add User (do not provide password) > Create account です。

![Invite User](../img/InviteUser.png)

管理者が配布できる共有リンクが表示されます。パスワード設定用メールも送信されます（メールが[サーバーで設定済み](#how-to-configure-smtp-settings-in-btcpay)の場合）。新規ユーザーは招待リンクへの初回アクセス時にパスワードを作成します。

### ユーザーの U2F と 2FA を無効化するには？

登録済みユーザー（例: `user@example.com`）の U2F/2FA 設定を削除するには、次のコマンドを実行します。

```bash
# In root
sudo su -
cd $BTCPAY_BASE_DIRECTORY/btcpayserver-docker/
# Disable U2F and 2FA
./btcpay-admin.sh disable-multifactor user@example.com
```

### BTCPay で SMTP 設定を行うには？

SMTP は各ストア設定で構成できます。管理者権限があればサーバー全体にも設定できます。

メールプロバイダーごとに設定が異なるため正確な値は案内できませんが、以下は Gmail の例です。

```
SMTP Host: smtp.gmail.com
SMTP Port: 587
SSL Protocol: ON
TLS Protocol: ON
SMTP Username: (your Gmail username)
SMTP Password: (your Gmail password)
```

Gmail では安全性の低いアプリからのアクセス許可が重要です。有効化は Manage Your Google Account > Security > Allow Less Secure Apps (On) です。この設定は未使用時に Google 側で自動的にオフになることがあります。SMTP が急に動かない場合はこの設定を確認してください。

Gmail アカウントで 2 段階認証を使っている場合は、[こちらの記事](https://support.google.com/mail/answer/185833)を参照してください。

BTCPay のテストメール機能で、メール送信が正常か確認してください。業務用途でより信頼性の高い SMTP を求める場合は、Mailgun のような専用メールサービスの利用を検討してください。

### エラー: `Maintenance feature requires access to SSH properly configured in BTCPayServer configuration`

Docker の問題により、BTCPay Server のメンテナンス機能設定が一時的に壊れる場合があります。通常は BTCPay Server を再起動すると解消します。ただしこのエラーが UI に表示されると再起動ボタンが無効になります。解決には [ssh で再起動](#how-to-restart-btcpay-server) してください。

### エラー: `Your local changes to the following files would be overwritten by merge`

誤ってファイルを編集してしまうと、次のエラーで更新機構が壊れる場合があります。

```bash
error: Your local changes to the following files would be overwritten by merge:
```

これを修正するには、[サーバーへ ssh 接続](#how-to-ssh-into-my-btcpay-running-on-vps) して次を実行します。

```bash
sudo su -
cd btcpayserver-docker
git reset --hard origin/master
```

### エラー: `BTCPAY_SSHKEYFILE is not set when running the docker install, or unable to update through Server Settings / Maintenance`

`docker-compose` 実行時（`btcpay-up.sh` または `btcpay-setup.sh`）に、次のようなメッセージが表示されることがあります。

```bash
WARNING: The BTCPAY_SSHKEYFILE variable is not set. Defaulting to a blank string.
WARNING: The BTCPAY_SSHTRUSTEDFINGERPRINTS variable is not set. Defaulting to a blank string.
```

`BTCPay Server` は、フロントエンドから次の操作を行うために SSH アクセスを必要とします。

- サーバー更新
- サーバーのドメイン名変更

次のコマンドで、SSH 経由のサーバーアクセスを BTCPay に付与できます。

```bash
sudo su -

# Checkout latest BTCPay Server version
cd $BTCPAY_BASE_DIRECTORY/btcpayserver-docker
git checkout $(git tag --sort -version:refname | awk 'match($0, /^v[0-9]+\.[0-9]+\.[0-9]+$/)' | head -n 1)

# Setup SSH access via private key
ssh-keygen -t rsa -f /root/.ssh/id_rsa_btcpay -q -P "" -m PEM
echo "# Key used by BTCPay Server" >> /root/.ssh/authorized_keys
cat /root/.ssh/id_rsa_btcpay.pub >> /root/.ssh/authorized_keys
BTCPAY_HOST_SSHKEYFILE=/root/.ssh/id_rsa_btcpay
. ./btcpay-setup.sh -i
```

## テーマ / カスタマイズ

### BTCPay テーマスタイルをカスタマイズするには？

BTCPay のテーマをカスタマイズする方法は2つあります。
簡単な方法は、[Theme documentation](../Development/Theme.md) にある通り、BTCPay 側でカスタムテーマ設定を選択または指定することです。

より高度なテーマ変更では、BTCPay リポジトリを fork してデザイン変更を適用する必要が出ることが多いです。Docker イメージをビルドして Docker Hub に公開し、`BTCPAY_IMAGE` 環境変数をあなたのイメージタグに設定（`export BTCPAY_IMAGE="your custom btcpay docker image"`）して、[BTCPay Docker](https://github.com/btcpayserver/btcpayserver-docker) で通常通りセットアップ（`. ./btcpay-setup.sh -i`）を実行します。生成された docker compose を、カスタムイメージを使うように修正してください。

:::warning
fork した BTCPay Server では、BTCPay の更新ごとに新しいイメージを手動作成して同じ手順を毎回実施する必要があります。基本的にはデフォルト設定とテーマオプションを使うことを推奨します。
:::

### チェックアウトページを変更するには？

BTCPay のチェックアウトページの見た目は、[instructions here](../Development/Theme.md#checkout-page-theme) に従うことで簡単に変更できます。

### BTCPay に Google Analytics コードを追加するには？

`~/wwwroot/checkout/js/core.js.` に GA コードを埋め込むことで実現できます。これは最も簡単な方法ですが、BTCPay を更新するたびに再適用が必要です。コードを fork して手動デプロイする手間は省けます。更新のたびに Docker 更新後、同じ行を `.js` ファイルへ再追加してください。

## ポリシー

### BTCPay Server で登録を許可するには？

他ユーザーにサーバー登録と利用を許可するには、Server Settings > Policies で registration を有効化します。SMTP を[正しく設定済み](#how-to-configure-smtp-settings-in-btcpay)であれば、スパムやボット登録を防ぐためにメール確認を要求できます。

### 検索エンジンに BTCPay Server を表示させないには？

Server Settings > Policies でサイトのインデックスを抑制すると、サーバーヘッダーに `<meta name="robots" content="noindex">` が追加され、検索エンジンにインデックスしないよう通知できます。

このリクエストを実際に反映するかは検索エンジン側に依存し、ページが完全に検索結果から消えるまで時間がかかる場合があります。正確な反映時期は制御できず、Google など各検索エンジンのクローラーボットに依存します。

## サービス

### BTCPay フルノードへリモート接続するには？

BTC-P2P 接続に対応した外部ウォレットを使っている場合、BTCPay フルノードへ簡単に接続できます。これにより、第三者サーバーへの情報漏えいを避け、自分のフルノードのみに依存できます。
対応する BTC-P2P ウォレットを接続するには、**Server Settings > Services > Full node P2P** で QR コードを表示し、ウォレットで読み取るか、コピー＆ペーストで入力してください。

![BTC-P2P](../img/BTC-P2P.png)

Services に Full node P2P が表示されない場合は、[サーバーで Tor を有効化](./Deployment.md#how-do-i-activate-tor-on-my-btcpay-server)する必要がある可能性があります。

## ファイル

### BTCPay にファイルをアップロードするには？

BTCPay Server インスタンスにファイルをアップロードするには、まず Server Settings > Services で External Storage を有効化し、利用するストレージプロバイダーを選択します。次に Server Settings > Files でローカルファイルを参照・アップロードします。ストレージシステムの制限によっては、大きなファイルのアップロードが難しい場合があります。
