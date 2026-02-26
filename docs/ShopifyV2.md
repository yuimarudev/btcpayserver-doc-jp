<!-- legacy-anchor-aliases -->
<span id="setup-a-shopify-app"></span>
<!-- /legacy-anchor-aliases -->

# ShopifyでBTCPay Serverを使ってBitcoin決済を受け付ける

Shopify向けBTCPay Serverのご紹介です。これはオープンソースの決済ゲートウェイで、手数料なしで、Webサイトや実店舗で顧客から直接Bitcoin決済を受け付けられます。

Shopifyとの連携により、セルフホストしたBTCPay Serverを[Shopifyストア](https://www.shopify.com/)に接続し、迅速かつ安全にBitcoin決済を受け付けられます。

## BTCPayでできること:

- **手数料ゼロ**: 手数料無料の決済ゲートウェイを利用できます。そうです、本当にゼロです。
- **直接入金、仲介業者やKYC不要**: 仲介や煩雑な書類作業は不要。資金を直接あなたのウォレットで受け取れます。
- **完全自動化システム**: 決済、請求書管理、返金をBTCPayが自動で処理します。
- **チェックアウトでBitcoin QRコードを表示**: かんたんで安全な支払い手段を提供し、顧客体験を向上します。
- **セルフホスト基盤**: 決済ゲートウェイを完全に自分でコントロールできます。
- **Lightning Network統合**: 即時・高速・低コストの入出金に対応します。
- **CSVエクスポートが簡単**
- **柔軟なプラグインシステム**: 必要に応じて機能拡張できます。
- **POS統合**: 実店舗でも決済を受け付けできます。
- **多言語対応**: そのままでグローバルな顧客に対応できます。
- **コミュニティ主導のサポート**: 専用コミュニティから迅速な支援を受けられます（[Mattermost](http://chat.btcpayserver.org/) または [Telegram](https://t.me/btcpayserver)）。

:::warning
これは旧Shopify V2ドキュメント（2024年12月30日〜2025年2月23日公開）の簡略版です。旧版では別VPSにShopifyアプリをデプロイする必要がありましたが、現在はアプリをBTCPay Server上に直接デプロイし、BTCPay Shopifyプラグインも変更されています。今後保守するのはこの新構成のみのため、すべてのユーザーがこの構成へ移行してください。
:::

## 前提条件

セットアップを始める前に、以下を用意してください。

- [Shopify](https://www.shopify.com/)アカウントとストア
- 有効なShopifyサブスクリプションプラン（最低でもBasic Shopify）
- [Shopifyパートナーアカウント](https://www.shopify.com/partners)（登録無料）
- BTCPay Server - [セルフホスト](/Deployment/) もしくは [サードパーティホスト](/Deployment/ThirdPartyHosting.md) で稼働している * v2.0.6 以降
- [BTCPay Serverストアの作成](CreateStore.md) と [ウォレット設定](WalletSetup.md)

* サードパーティホストを利用している場合、ホスト側で Shopify fragment の有効化と Shopify v2 プラグインの有効化が必要です。これが無効だと BTCPay Server 上に表示されません。

## Shopify アプリを作成する

まず、Shopify Partner ポータルで新しいアプリを作成します。[Shopifyパートナー](https://www.shopify.com/partners)に登録済みであることを確認してください（登録無料）。

1. Shopify Partner の[ダッシュボード](https://partners.shopify.com)で `App distribution` をクリックし、次のページで `View Dev Dashboard` をクリックします（新しいブラウザタブで開きます）。開いたページで `Create app` > `Start from Dev Dashboard` をクリックし、アプリ名（例: BTCPay Server）を入力して `Create` をクリックします。

   ![Shopify-App: Create app manually](./img/shopifyv2/create_app_manually.png)

2. 作成後、`Settings` をクリックすると「Client ID」と「Client secret」が表示されます。後で使用するので控えてください。
   ![Shopify-App: client id and secret](./img/shopifyv2/partner-app_client-id-secret.png)
3. パートナーダッシュボードに戻り、ページを再読み込みして新しいアプリが一覧に表示されることを確認します。`All Apps` をクリックし、作成したアプリを選択、左メニューの `API access requests` を開きます。下にスクロールして `Allow network access in checkout and account UI extensions` でネットワークアクセスを許可します。許可後、以下の画面になります。
   ![Shopify-App: Partner app network access](./img/shopifyv2/partner_app_network_access.png)

:::tip
ネットワークアクセス付与時に "Could not grant checkout ui extension scope 'read_checkout_external_data'" というエラーが出る場合、パートナーアカウントプロフィールの姓・名が未設定であることが原因です。プロフィールに必要情報を入力してから再度付与してください。
:::

4. Shopifyパートナーロゴをクリックしてパートナーダッシュボードに戻ります。
5. ページ下部の "Settings" をクリックします。  
   ![partners_settings.png](./img/shopifyv2/partners_settings.png)
6. 下部の "CLI Token" セクションまでスクロールし、"Manage tokens" をクリックします。
   ![partners_settings-cli-token.png](./img/shopifyv2/partners_settings-cli-token.png)
7. 右上の "Generate new token" をクリックします。
   ![partners_create-cli-token.png](./img/shopifyv2/partners_create-cli-token.png)
8. モーダルポップアップではデフォルトのまま "Generate token" をクリックします。
   ![partners_create-cli-token-duration.png](./img/shopifyv2/partners_create-cli-token-duration.png)
9. トークンをコピーし、先ほどの `Client ID` と `Client secret` と一緒に控えます。このトークンは一度しか表示されません。
   ![partners_cli-token-created.png](./img/shopifyv2/partners_cli-token-created.png)

## BTCPay Server に Shopify-BTCPay アプリをデプロイする

:::tip
以下の手順を実行するには、自分の BTCPay Server インスタンスの管理者である必要があります。サードパーティホストの場合は、ホスト管理者が Shopify fragment を有効化し、Shopify v2 プラグインを有効化していないと利用できません。
:::

### Shopify フラグメントをデプロイする

1. SSHでBTCPay Serverにログインします。
2. 次のコマンドを実行します。

```bash
# if you are not root user, switch to root
sudo su -

# go to the BTCPay Server docker directory
cd $BTCPAY_BASE_DIRECTORY
cd btcpayserver-docker

# make sure you have latest btcpayserver-docker commits
git pull

# add the shopify fragment to your BTCPay Server
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-add-shopify"

# run the setup script
. ./btcpay-setup.sh -i
```

セットアップスクリプトは [BTCPay Shopify app](https://github.com/btcpayserver/shopify-app) を取得して Docker コンテナへデプロイします。動作確認する場合は次を実行してください。

```bash
docker ps | grep shopify
```

これでBTCPay Server Shopifyプラグインのインストールに進めます。

### BTCPay Server Shopify プラグイン v2 をインストールする

1. BTCPay Server でサイドバーの "Manage Plugins" を開き、btcpayserver 製 "BTCPay Server Shopify plugin v2" をインストールします。
![plugin_install.png](./img/shopifyv2/plugin_install.png)
2. 続いて上部に戻り、"Restart now" リンクをクリックしてBTCPayを再起動します。
![plugin_install-restart.png](./img/shopifyv2/plugin_install-restart.png)

BTCPay Server の再起動には数分かかることがあります。 

### BTCPay Server Shopify プラグインを設定する
1. 上部で対象ストアが選択されていることを確認し、左サイドバーの `Shopify v2` をクリックします。
2. 3セクションのうち最初のセクションで、上記[Shopify アプリの作成手順](#setup-a-shopify-app)の `Client ID` と `Client Secret` を入力し、"Save" をクリックします。
   ![plugin_section-1.png](./img/shopifyv2/plugin_section-1.png)
3. 次の "Deploy the app" セクションで `App name` を入力します（[Shopify アプリの作成手順](#setup-a-shopify-app)と同じ名前を推奨）。`CLI token` フィールドには先ほど控えた "CLI Token" を入力します。
   ![plugin_section-2.png](./img/shopifyv2/plugin_section-2.png)
4. "Deploy App" をクリックします。
   コンソール出力が表示され、アプリがShopifyへデプロイされます。正常に完了すると、このセクションは閉じて最後のセクションが開きます。
   ![plugin_section-2--console-output.png](./img/shopifyv2/plugin_section-2--console-output.png)
5. 最後の "Install the app on your Shopify store" セクションでは操作は不要です。ストアにアプリがインストール済みかを確認するだけで、実際のインストールは次の手順で行います。
   ![plugin_section-3.png](./img/shopifyv2/plugin_section-3.png)

## Shopify ストアに BTCPay-Shopify アプリをインストールする

次に、Shopify アプリを Shopify ストアへインストールします（これでストアが BTCPay Server にリンクされます）。

1. [パートナーアカウント](https://partners.shopify.com/)のアプリ概要で、作成したアプリを選択し、`Choose Distribution` をクリックして `Custom distribution` を選びます。確認して確定します。
    :::tip
    `Custom distribution` を選ぶと、そのアプリは1つのShopifyストアでしか使えません。これは元に戻せません。複数ストアがある場合は、ストアごとに複数アプリをデプロイしてください。
    :::
    ![App install: select custom distribution](./img/shopifyv2/app-deploy_custom-distribution-1.png)
    ![App install: confirm custom distribution](./img/shopifyv2/app-deploy_distribution-confirm.png)

2. 次の画面で、アプリを紐づけたいShopifyストアのURLを入力します。通常はストア設定時に表示される内部ストアURL（例: `your-store.myshopify.com`）です。  
   ![app-deploy_copy-store-url.png](./img/shopifyv2/app-deploy_distribution-copy-store-url.png)
   "Allow multi-store install for one Plus organization" のチェックを外してください。  
   ![App install: enter your store url](./img/shopifyv2/app-deploy_distribution-generate-link.png)
3. `Generate link` をクリックするとリンクが生成されます。リンクをコピーし、ブラウザで開いてインストールを開始します。
   ![App install: link generated](./img/shopifyv2/app-deploy_distribution-generated-link-copy.png)
4. アプリが一覧表示されるので `Install` をクリックしてインストールします（未ログインの場合は先にログインが必要です）。
   ![app-deploy_distribution-install-to-store-confirm.png](./img/shopifyv2/app-deploy_distribution-install-to-store-confirm.png)
:::tip
アプリ画面には顧客情報やストアオーナー情報へのアクセス権が表示されますが、実際にはそれらのデータへアクセスしません。アプリが使うのは注文ステータス更新のための checkout ID と order ID のみで、顧客や管理者の個人データが BTCPay Server へ送信されることはありません。
:::
5. インストールが完了すると、"Shopify plugin successfully configured" というメッセージが表示されるアプリページが開きます。
   ![app-deploy_install-successful.png](./img/shopifyv2/app-deploy_install-successful.png)
6. （任意）最下部の "You can navigate to your plugin's settings page by clicking here." リンクをクリックすると、アプリが BTCPay Server に正しく接続されているか再確認できます（最後のセクションも緑チェックになります）。
   ![app-deploy_plugin-all-green.png](./img/shopifyv2/app-deploy_plugin-all-green.png)

## "Thank you" ページをカスタマイズする

1. Shopifyダッシュボードで左ナビ下部の `Settings` をクリックし、`Checkout` を選択後、`Customize` をクリックします。  
   ![Store steup: settings](./img/shopifyv2/app-setup_step-4-1.png)
   ![Store setup: settings customize checkout](./img/shopifyv2/app-setup_step-4-2.png)
2. エディタ内で表示ページを "Thank you" ページに切り替えます。  
   ![Store setup: switch to thank you page](./img/shopifyv2/app-setup_step-5-1.png)   
   ![Store setup: thank you page](./img/shopifyv2/app-setup_step-5-2.png)
3. 左パネルの `Apps` アイコンをクリックします。
   ![App Setup: Step 9](./img/shopifyv2/app-setup_step-6.png)
4. 一覧にある "BTCPay Checkout" アプリの (+) をクリックし、続いて表示される "Thank you" ページを選択します。
   ![App Setup: Step 10](./img/shopifyv2/app-setup_step-7.png)
5. 拡張機能が "Thank you" ページに追加されます。**重要**: 右上の "Save" を必ずクリックしてください。
   ![App Setup: Step 11](./img/shopifyv2/app-setup_step-8.png)
6. 正常動作の確認として、"BTCPay Checkout" の横にある左向き矢印 `<` をクリックし、"Order details section" に表示されていることを確認します。
   ![App Setup: Step 12.1](./img/shopifyv2/app-setup_step-9-1.png)
   ![App Setup: Step 12.2](./img/shopifyv2/app-setup_step-9-2.png)

## Shopify でカスタム決済方法を設定する

最後に、顧客に Bitcoin 決済オプションを表示するため、Shopify でカスタム決済方法を設定します。

1. ダッシュボードに戻り、左サイドバーの `Settings` >> `Payments` をクリックします。"Manual payment methods" までスクロールし、`(+) Manual payment method` をクリック、ドロップダウンで `Create custom payment method` を選択します。
   ![Create payment method step 1](./img/shopifyv2/pm_step_1.png)
2. `Custom payment method name` に `Pay with Bitcoin (BTCPay Server)` のような名称を入力します（下の注記も参照）。他の項目は任意ですが入力は必須ではありません。
   なお、Bitcoin 支払いはチェックアウト直後ではなく "Thank you" ページの次画面で行われることを顧客に案内する必要があります。理想的には `Additional details` フィールドで案内します。
   "Thank you" ページで決済オプションが表示されるまで若干遅延する場合があるため、その旨も顧客に伝えることを推奨します。推奨文言: `「Thank you」ページに Bitcoin 支払いボタン「Complete payment」が表示されます。支払い完了のため、このボタンをクリックしてください。`
   :::tip
   "Custom Payment method name" には次のいずれかの語を必ず1つ以上含めてください（大文字小文字は区別されません）: `bitcoin`, `btcpayserver`, `btcpay server` または `btc`。
   :::
3. `Activate` をクリックすると、Shopify と BTCPay Server の決済方法設定は完了です。
   ![Create payment method step 2 and 3](./img/shopifyv2/pm_step_2_and_3.png)

おめでとうございます。BTCPay-Shopifyアプリのインストールと、Shopifyストアでの決済方法設定が完了しました。利用準備は完了です。以下にデモのチェックアウトフローを示します。

## 設定完了後のデモチェックアウトフロー

顧客がチェックアウトページで決済方法を選択:
![Shopify checkout show payment option](./img/shopifyv2/payment_option.png)

顧客は "Thank you" ページへリダイレクトされ、支払いボタンが表示:
![Shopify Thank you page with pay button](./img/shopifyv2/complete_payment.png)

顧客が "Complete payment" をクリックし、BTCPay支払いページへリダイレクト:
![Customer click on complete payment](./img/shopifyv2/btcpay_checkout.png)

顧客が請求書を支払い、マーチャントへ戻るをクリック:
![Customer paid invoice](./img/shopifyv2/invoice_paid.png)

顧客はShopifyの注文ステータスページへリダイレクト:
![Order status page after payment](./img/shopifyv2/order_status_page_after_payment.png)

---

BTCPay Server のストア側では、支払い済み請求書を確認できます:
![BTCPay Server shopify step 37](./img/shopifyv2/paid_invoice_btcpay.png)

クリックすると支払い詳細を確認できます:
![BTCPay Server shopify step 38](./img/shopifyv2/invoice_payment_details.png)

## FAQ

- Shopify V1とShopify V2を同時に使えますか？ いいえ、同時利用は推奨されません。両方を同時に使うと、Shopify側で注文レコードが重複するなど予期しない動作が発生する可能性があります。Shopify V1は無効化し、Shopify V2のみを使ってください。

- 請求書がInvalidになったらどうなりますか？ 有効期限までに確認された支払いの合計がShopifyに反映されます。

- BTCPay Serverで請求書をInvalidに手動変更したらどうなりますか？ BTCPay側は何もしないため、Shopify注文は保留のままになります。

- BTCPay Serverで請求書を手動でsettledにしたらどうなりますか？ Shopify注文は「支払い完了」としてマークされます。

- 顧客が支払わなかった場合は？ BTCPay請求書の有効期限切れ時にShopify注文は無効化され、在庫が戻されます。

- 顧客が支払ったが、確認が妥当な時間で進むには手数料不足だった場合は？ BTCPay請求書はInvalidとなり、Shopify注文はPayment Pendingのままです。

- 顧客が一部だけ支払った場合は？ BTCPay請求書は期限切れになります。期限時点で確認済み支払いがある場合はShopify注文はPartially Paid、ない場合はPayment Pendingのままになります。

- 部分支払いを避けるには？ 部分支払いは、取引所から支払った際に手数料が差し引かれることで発生しがちです。対策として、ストア設定で小さな[underpayment tolerance](https://docs.btcpayserver.org/FAQ/Stores/#consider-the-invoice-paid-even-if-the-paid-amount-is-less-than-expected)を設定してください。

- 顧客が支払いを完了しなかった場合、請求書リンクを再共有するには？ BTCPay Server は、BTCPay が選択された決済方法であれば請求書リンクを Shopify 注文の metafields に保存します。

Shopify API で metafields を取得します:
```pwsh
https://{SHOPNAME}.myshopify.com/admin/api/{VERSION}/orders/{ORDER-ID}/metafields.json
```
詳細:
1. [Shopify GraphQL API - Order Metafields](https://shopify.dev/docs/api/admin-graphql/latest/objects/Order#fields-metafields)
2. [Shopify REST API - Order Metafields](https://shopify.dev/docs/api/admin-rest/2025-01/resources/metafield#get-orders-order-id-metafields). 

## トラブルシューティング

### プラグインのインストールが失敗する場合

BTCPay Server でプラグインをインストールしようとした際に "This app has been migrated to the new Next-Gen Dev platform" エラーが表示される場合、BTCPay インスタンス管理者であれば、前述の `Deploy the Shopify fragment` の手順どおり BTCPay Server を更新すれば解決できます。管理者でない場合は、管理者に Shopify fragment の更新を依頼してください。更新後はこのガイドに従って BTCPay Server と Shopify を正常に連携できるはずです。

![Shopify Migration - Plugin Install fails](./img/shopifyv2/plugin_install_fails.png)

### アプリ更新時にエラーが出る場合

2025年9月15日より前にアプリをインストールしていて、再デプロイで更新しようとした際に "Your app has extensions which need to be assigned 'uid' identifiers" エラーが出る場合、[Shopify app update](https://shopify.dev/docs/apps/build/dev-dashboard/migrate-from-partners)の大きな変更が原因です。アプリをアンインストールして、新しく作り直す必要があります。以下を実施してください。

 - Shopify ストアからアプリをアンインストール
 - Shopify Partnersでアプリを削除（任意）
 - BTCPay Server の Shopify プラグイン画面に戻り、`Reset` ボタンをクリック
 - `btcpayserver-docker` ディレクトリで `./btcpay-update.sh` を実行し、最新の Shopify fragment（>= 1.5）を使用していることを確認
 - インストール手順を最初からやり直す

![Shopify Migration - Old user error](./img/shopifyv2/shopify_migration_old_user_error.png)

### BTCPay Shopify app を更新するには？

[BTCPay Shopify app](https://github.com/btcpayserver/shopify-app)が更新されたときは、次の手順で新バージョンをストアへ反映できます。

まず、[Shopify Partner ポータル](https://partners.shopify.com) で新しい CLI トークンを取得します（手元にない、または期限切れの場合）。

#### 新しい CLI トークンを取得する
1. Shopify Partner Portal にログイン
2. ページ下部の "Settings" をクリック  
   ![Partner dashboard Settings](./img/shopifyv2/partners_settings.png)
3. 下部の "CLI Token" セクションで "Manage tokens" をクリック
   ![Settings CLI Token](./img/shopifyv2/partners_settings-cli-token.png)
4. 右上の "Generate new token" をクリック
   ![Create CLI Token](./img/shopifyv2/partners_create-cli-token.png)
5. モーダルではデフォルトのまま "Generate token" をクリック
   ![Select CLI Token validity duration](./img/shopifyv2/partners_create-cli-token-duration.png)
6. CLI トークンをコピー（このトークンは一度しか表示されません）
   ![CLI Token was created successfully](./img/shopifyv2/partners_cli-token-created.png)

#### BTCPay Server に SSH 接続する

次に BTCPay Server へログインし、更新スクリプトを実行して最新 Docker イメージ（BTCPay Shopify app を含む）を取得します。

```bash
# if you are not root user, switch to root
sudo su -

# go to the BTCPay Server docker directory
cd $BTCPAY_BASE_DIRECTORY
cd btcpayserver-docker

# run the update script
./btcpay-update.sh
```

#### BTCPay Shopify app を更新する

1. BTCPay Server にログイン
2. Shopify と接続済みのストアを選択
3. 左サイドバーの `Shopify v2` をクリック
4. 2つ目の "Deploy the app" セクションを展開
5. CLI トークンを貼り付けて "Deploy App" をクリック
![plugin_update-app--deploy.png](./img/shopifyv2/plugin_update-app--deploy.png)
6. コンソール出力が表示され、正常終了するとセクションが閉じます
![plugin_update-app--console.png](./img/shopifyv2/plugin_update-app--console.png)
![plugin_update-app--finished.png](./img/shopifyv2/plugin_update-app--finished.png)

これで BTCPay Shopify app の更新は完了です。

### Shopify Partner ポータルで新しいアプリを作成できない

右上プロフィールが "null null" と表示されていないか確認してください。これはプロフィールの姓・名が未入力の状態です。以下の手順で入力すると解消します。
1. 右上のプロフィールをクリック
2. "Your profile" を選択
3. "First name" と "Last name" を入力
4. Shopify Partners に戻ると作成できるようになります

## サポートとコミュニティ

サポートが必要な場合や質問がある場合は、[Mattermost](https://chat.btcpayserver.org/) または [Telegram](https://t.me/btcpayserver) のサポートチャネルへ参加してください。
