# (1) アカウント登録

このページは、自分の BTCPay Server インスタンスでアカウントを登録する場合、またはサードパーティホストを利用する場合に関する内容です。

デモアカウントを登録するには、[official demo](https://mainnet.demo.btcpayserver.org/login) を開いてください。

自分のインスタンスをデプロイするには、[choosing a deployment method](/Deployment/README.md) を参照してください。

サードパーティホストの一覧（完全版ではありません）は、BTCPay Server の [directory](https://directory.btcpayserver.org/filter/hosts) で確認できます。

## アカウント登録

BTCPay Server のセットアップ最初の手順は、ユーザーアカウントの作成です。新規デプロイされた BTCPay Server で**最初に作成されたアカウント**は自動的に **admin** になります。

登録するには、BTCPay Server の URL を開き、右側の登録フォームに入力します。パスワード、パスワード確認、e-mail を入力して "Register" をクリックしてください。自動的にログインされます。[third-party host](/Deployment/ThirdPartyHosting.md) を利用している場合、登録確認のため e-mail アドレスの確認を求められることがあります。

![BTCPay Server registration](./img/btcpay-registration-page.png)


アカウント作成が完了すると、新しいページへリダイレクトされ、ストアを設定できます。ここでは、ストア名の入力、デフォルト通貨の選択、為替レートプロバイダーの選択が必要です。

![BTCPay Server registration](./img/btcpay-registration-store-creation.png)

![First Store Creation](./img/FirstStoreCreation.png)


おめでとうございます。最初のステップである、BTCPay Server でのアカウントとストアの作成が完了しました。

Next Step: - [Connecting a Wallet](./WalletSetup.md).

サーバーインスタンスで複数ストアを管理する必要がある場合は、[Creating a store](./CreateStore.md) を参照してください。


### e-mail の設定

サーバー管理者は [SMTP settings](./FAQ/ServerSettings.md#how-to-configure-smtp-settings-in-btcpay) を設定することを推奨します。e-mail を設定すると、資格情報を忘れた場合にインスタンス利用者が簡単にパスワードをリセットできます。

他のユーザーがサーバーへアクセスできるようにするには、Server Settings > Policies で登録を有効化する必要があります。

### 二要素認証

セキュリティをさらに高めてアカウントを保護するため、二要素認証（2FA と U2F の両方に対応）を有効化することを推奨します。2FA または U2F を有効にするには、ヘッダーメニューのユーザー設定アイコンをクリックしてください。
