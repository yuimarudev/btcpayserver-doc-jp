---
description: WooCommerce ストアに BTCPay Server を統合する方法。
tags:
  - WooCommerce
  - WordPress
  - プラグイン
  - eコマース
---
<!-- legacy-anchor-aliases -->
<span id="41-global-settings"></span>
<span id="btcpay-order-statuses"></span>
<!-- /legacy-anchor-aliases -->


# WooCommerce 連携

このドキュメントでは、**WooCommerce ストアに BTCPay Server を統合する方法**を説明します。
まだストアがない場合は、[このステップバイステップの記事](https://web.archive.org/web/20221003083329/https://bitcoinshirt.co/how-to-create-store-accept-bitcoin/5/)を参考に、ゼロから作成してください。

:::tip 注記
このガイドは BTCPay for WooCommerce V2 プラグインを対象としています。現在メンテナンスされていない旧プラグイン（BitPay API ベース）の手順は[こちら](https://github.com/btcpayserver/btcpayserver-doc/blob/cba96292ceea9483711ab53c479a98357383f857/docs/WooCommerce.md)で確認できます。
:::

[[toc]]

![BTCPay - WooCommerce インフォグラフィック](./img/infographics/BTCPayInfographic.png)

既存の WooCommerce ストアに BTCPay Server を統合するには、以下の手順に従うか、次の動画を視聴してください。

[![BTCPay - WooCommerce](https://img.youtube.com/vi/ULcocDKZ1Mw/mqdefault.jpg)](https://www.youtube.com/watch?v=ULcocDKZ1Mw)

## 要件

このプラグインをインストールする前に、以下の要件を満たしていることを確認してください。

- PHP バージョン 8.0 以上
- cURL、gd、intl、json、mbstring の PHP 拡張が利用可能
- WooCommerce サイト（[インストール手順](https://woocommerce.com/document/installing-uninstalling-woocommerce/) または [BTCPay Server 上で直接デプロイ](#deploying-woocommerce-from-btcpay-server)）
- BTCPay Server バージョン 1.3.0 以上を利用している（[セルフホスト](/Deployment/README.md) または [サードパーティホスト](/Deployment/ThirdPartyHosting.md)）
- [インスタンスに登録済みアカウントがある](./RegisterAccount.md)
- [インスタンスに BTCPay ストアがある](./CreateStore.md)
- [ストアにウォレットが接続されている](./WalletSetup.md)

## 1. BTCPay プラグインのインストール

**BTCPay for WooCommerce V2 プラグイン**をダウンロードする方法は 3 つあります。

- WordPress の管理ダッシュボードから（推奨、以下参照）
- [WordPress Repository](https://wordpress.org/plugins/btcpay-greenfield-for-woocommerce/)
- [GitHub Repository](https://github.com/btcpayserver/woocommerce-greenfield-plugin/releases)

### 1.1 WordPress 管理ダッシュボードからプラグインをインストール（推奨）

1. WordPress > Plugins > Add New（新規追加）。
2. 検索欄に「BTCPay V2」と入力。
3. インストールして有効化。

![BTCPay WordPress V2: プラグインのインストール](./img/woocommerce/btcpay-wc-2--01-plugin-search.png)

### 1.2 GitHub からプラグインをダウンロードしてインストール

[最新の BTCPay プラグインをダウンロード](https://github.com/btcpayserver/woocommerce-greenfield-plugin/releases)し、`.zip` 形式で WordPress サイトにアップロードして有効化します。

[![BTCPay Server Woo プラグイン](https://img.youtube.com/vi/6QcTWHRKZag/mqdefault.jpg)](https://www.youtube.com/watch?v=6QcTWHRKZag)

## 2. WooCommerce と BTCPay Server の接続

BTCPay for WooCommerce V2 プラグインは、**BTCPay Server（決済プロセッサ）と e コマースストアをつなぐブリッジ**です。
セルフホストでもサードパーティホストでも、接続手順は同じです。

通知リンクの「**please configure the plugin here**（ここでプラグインを設定してください）」（以下のスクリーンショット参照）をクリックするか、次の手順で開けます。

- ストアのダッシュボードへ移動
- WooCommerce > Settings
- [BTCPay Settings] タブをクリック

![BTCPay WordPress V2: BTCPay Settings へのリンク](./img/woocommerce/btcpay-wc-2--02-activated-configure.png)

<a id="21-connect-using-the-api-key-wizard--recommended-"></a>
### 2.1 API キーウィザードを使って接続（推奨）

1. 「**BTCPay Server URL**」欄にホストの完全 URL（https を含む）を入力します。例: https://btcpay.mydomain.com
2. [Generate API key] ボタンをクリックします（BTCPay Server の「Authorization request」ページにリダイレクトされます）。
   ![BTCPay WordPress V2: BTCPay Settings へのリンク](./img/woocommerce/btcpay-wc-2--03-settings--api-key-redirect.png)

3. BTCPay Server インスタンスにログインしていない場合は、ここでログインします（任意）。
   ![BTCPay WordPress V2: BTCPay Server へログイン](./img/woocommerce/btcpayWooLmode1.jpg)
4. 接続したいストアを選択します（ストアが 1 つだけの場合は自動選択されます）。
   ![BTCPay WordPress V2: ストアを選択](./img/woocommerce/btcpay-wc-2--05-api-auth-select-store.png)
5. 必要な権限はすでに入力済みなので、[Authorize app] をクリックするだけです。
   ![BTCPay WordPress V2: authorize app をクリック](./img/woocommerce/btcpay-wc-2--06-api-auth-authorize-button.png)
6. WooCommerce ストアに戻ると、API キーと Store ID が自動入力されます。さらに webhook も自動作成されます。「Webhook status」欄に「Webhook setup automatically.」に続いて ID が表示されることを確認してください。
   ![BTCPay WordPress V2: プラグイン設定にリダイレクト後](./img/woocommerce/btcpay-wc-2--07-api-auth-after-redirect-prefilled.png)
7. 追加設定の前に、すべてが正しく保存されるよう **[Save]** をクリックしてください。
   ![BTCPay WordPress V2: Webhook 作成済み](./img/woocommerce/btcpay-wc-2--08-api-auth-save-webhook-created.png)

これでほぼ完了です。チェックアウトで Bitcoin 決済ゲートウェイを表示するには、サイドバーの「WooCommerce」->「Settings」から「Payments」タブを開き、「BTCPay (default)」決済ゲートウェイを有効化してください。

期待通り動作することを確認するため、以下の「3. チェックアウトのテスト」に進んでください。

<a id="22-connect-by-manually-creating-the-api-key-and-permissions"></a>
### 2.2 API キーと権限を手動作成して接続

前のセクションで説明したウィザードが使えない場合は、API キーを手動で作成することもできます。

1. 左下の _[Account]_ -> _Manage Account_ をクリック
   ![BTCPay WordPress V2: Manage Account](./img/woocommerce/btcpayWooLmode2.jpg)
2. _"API Keys"_ タブへ移動
3. 権限を選択するため _[Generate Key]_ をクリック
   ![BTCPay WordPress V2: API Keys overview](./img/woocommerce/btcpayWooLmode3.jpg)
4. 次の権限について _"Select specific stores"_ リンクをクリックします: `View invoices`, `Create invoice`, `Modify invoices`, `Modify stores webhooks`, `View your stores`, `Create non-approved pull payments`（返金で使用）
   ![BTCPay WordPress V2: API Keys Permissions](./img/woocommerce/btcpayWooLmode4.jpg)
5. _[Generate API Key]_ をクリック
   ![BTCPay WordPress V2: API Keys Save](./img/woocommerce/btcpayWooLmode5.jpg)
6. 生成された API Key を WordPress の _BTCPay Settings_ フォーム（Advanced settings）にコピー
   ![BTCPay WordPress V2: API Key をコピー](./img/woocommerce/btcpayWooLmode6.jpg)
7. store ID を WordPress の _BTCPay Settings_ フォーム（Advanced settings）にコピー
   ![BTCPay WordPress V2: Store ID をコピー](./img/woocommerce/btcpay-wc-2--7-man-api--copy-store-id.png)
8. BTCPay Settings フォームで以下を設定:
- _BTCPay Server URL_ を入力（先ほど API キーを作成した BTCPay Server インスタンスの URL）
- _BTCPay Server API Key_ と _Store ID_ を入力するため「Advanced settings」チェックボックスをオン（_Webhook secret_ は空のまま）
- ページ下部の _[Save]_ をクリック
  ![BTCPay WordPress V2: BTCPay Settings フォームを保存](./img/woocommerce/btcpay-wc-2--15-man-api--btcpay-settings-fill.png)
9. 通知「_BTCPay Server: Successfully registered a new webhook on BTCPay Server_」が表示され、_Setup status_ と _Webhook status_ が緑色になっていることを確認してください。
   ![BTCPay WordPress V2: BTCPay Settings フォーム保存後](./img/woocommerce/btcpay-wc-2--15-man-api--btcpay-settings-save.png)

これでほぼ完了です。チェックアウトで Bitcoin 決済ゲートウェイを表示するには、サイドバーの「WooCommerce」->「Settings」から「Payments」タブを開き、「BTCPay (default)」決済ゲートウェイを有効化してください。

期待通り動作することを確認するため、以下の「3. チェックアウトのテスト」に進んでください。

## 3. チェックアウトのテスト

ストアで少額のテスト購入を行うと安心です。
本番運用前に、すべてが正しく設定されていることを必ず確認してください。
最後の動画では、Electrum ウォレットでギャップリミットを設定し、チェックアウト処理をテストする手順を案内しています。

[![BTCPay Server Checkout](https://img.youtube.com/vi/Fi3pYpzGmmo/mqdefault.jpg)](https://www.youtube.com/watch?v=Fi3pYpzGmmo)

## 4. BTCPay WooCommerce V2 のカスタマイズ

### 4.1 グローバル設定

場所: _WooCommerce -> Settings -> Tab [BTCPay Settings]_

**BTCPay Server URL**

プロトコルを含む BTCPay Server インスタンスの URL。例: `https://btcpay.yourdomain.com`。

**BTCPay API Key（BTCPay APIキー）**

あなたの APIキー（前の手順で自動生成されたもの）。

**Store ID（ストアID）**

BTCPay Server ストアの Store ID。ストア設定ページで確認できます。

**Default Customer Message（既定の顧客メッセージ）**

チェックアウトで BTCPay 決済ゲートウェイを選択した後に表示される顧客メッセージをここでカスタマイズできます。「Separate payment gateways」オプションを使う場合は、各ゲートウェイ設定で上書きできます。

**Invoice pass to "Settled" state after（"Settled" とみなすまでの承認数）**

支払いを完全入金（settled）とみなす承認数を設定します。デフォルトでは BTCPay ストア設定の値が使用されます。

**BTCPay Order Statuses（BTCPay 注文ステータス）**

ビジネスモデルやストア設定に応じて、注文ステータスの調整が必要になる場合があります。
BTCPay が WooCommerce の特定の注文ステータスを自動でトリガーするよう設定できます。

- _New_ - 注文済み、未払い。
- _Paid_ - 支払い済みだが、ブロックチェーン上の承認数がまだ不足。
- _Settled_ - 支払い済みで、ブロックチェーン上で承認済み。
- _Settled (paid over)_ - 支払い済みで、ブロックチェーン上で承認済み、かつ過払い。
- _Invalid_ - 支払い済みだが、BTCPay ストア設定で定義された時間内に十分な承認数に達しなかった、または手動で無効化された。
- _Expired_ - 請求書の期限切れ、未払い。
- _Expired with partial payment_ - 請求書が期限切れで一部のみ支払い。

これらのステータスをどう自動化したいか、時間をかけて検討してください。
特定の BTCPay ステータスで WooCommerce 注文ステータスを変更したくない場合は、デフォルトの「- no mapping / defaults -」のままにできます。

注: デジタル商品と物理商品を販売している場合は、「Settled」の注文ステータスを「- no mapping / defaults -」のままにしておくべきです。デジタル商品のみを含む注文では、WooCommerce が「Processing」を自動でスキップし、直接「Completed」に進むためです。

別の例として、加盟店が「支払いは受領したが、処理は承認後に行う」ことを顧客へメール通知したい場合、「Paid」の注文ステータスを「On hold」に設定する必要があります。そのうえで WooCommerce 側で「On hold」ステータス時のメールをカスタマイズしてトリガーする必要があります。

最適な設定を見つけるには時間がかかるため、本番運用前に十分テストすることを推奨します。

**Modal checkout（モーダルチェックアウト）**

このオプションを有効にすると、BTCPay Server の請求書がチェックアウトページに直接表示されます（BTCPay Server インスタンスへ顧客をリダイレクトしません）。

**Separate Payment Gateways（決済ゲートウェイ分離）**

このオプションを有効にすると、BTCPay Server でサポートされる決済手段ごとに個別の決済ゲートウェイが生成されます。例えば、BTCPay Server ストアで BTC、LightningNetwork、Liquid Assets が有効なら、それぞれ個別ゲートウェイとして利用できます。これにより、ゲートウェイ別割引や国別制限など多くの新しいユースケースに対応できます。詳細は[こちら](./FAQ/Integrations.md#how-to-configure-additional-token-support)。

**Send customer data to BTCPayServer（顧客データ送信）**

デフォルトでは、メールアドレス以外の顧客データは BTCPay Server に送信されません。顧客住所データも送信したい場合は、ここで有効化できます。

**Debug Log（デバッグログ）**

問題が発生して詳細情報が必要な場合に役立ちます。ログは WooCommerce -> Status -> Log で確認できます。デバッグ後は必ず無効化してください。無効化しないとログが増え続け、ファイルシステムを圧迫します。

### 4.2 決済ゲートウェイ固有設定

前述の「Separate Payment Gateways」が有効かどうかに応じて、_WooCommerce -> Settings -> Tab [Payments]_ の決済ゲートウェイ設定に 1 つ以上のゲートウェイが表示されます。

すべての決済ゲートウェイで以下のオプションを設定できます。

**Title（タイトル）**
チェックアウトページに表示される決済ゲートウェイ名。デフォルトは「BTCPay (Bitcoin, Lightning Network, ...)」。

**Customer Message（顧客向けメッセージ）**

BTCPay 決済ゲートウェイ選択後に表示されるメッセージをカスタマイズできます。

**Gateway Icon（ゲートウェイアイコン）**

チェックアウト中に決済ゲートウェイの横へ表示するカスタムアイコンをアップロードまたは選択できます。デフォルトは BTCPay ロゴです。

#### 4.2.1 BTCPay (default)

デフォルト決済ゲートウェイでのみ使える追加オプション:

**Enforce payment tokens（paymentトークンのみに限定）**

BTCPay Settings で「Separate Payment Gateways」を有効化している場合、このオプションで payment トークンのみを強制できます。つまり、作成される請求書には「payment」タイプのトークンのみが含まれ、「promotion」タイプは含まれません。トークンタイプの違いは[こちら](./FAQ/Integrations.md#how-to-configure-additional-token-support#token-types)を参照してください。

#### 4.2.2 Separate Payment Gateways

個別決済ゲートウェイでのみ使える追加オプション（この機能が有効な場合）:

**Token Type（トークンタイプ）**

デフォルトでは「payment」タイプが選択されます。ただし、独自発行アセット/トークン（例: バウチャー用途）を含む Liquid Assets を使う場合は、ここで「promotion」を選択できます。これらは通常の payment トークンとは異なる方法で処理されます。詳細は[こちら](./FAQ/Integrations.md#how-to-configure-additional-token-support#promotional-tokens-100-discount)で確認できます。

## トラブルシューティング

### エラー: Call to undefined function BTCPayServer\Http\curl_init()

PHP バージョンが cURL 拡張に対応していることを確認してください（上記要件のとおり）。Debian/Ubuntu では `sudo apt install php-curl` でインストールできます。

### 請求書が支払い済みなのに注文ステータスが更新されない

まず、BTCPay Server のストア設定で webhook が作成されているか確認してください。webhook がない場合は、WooCommerce ストアの BTCPay Settings タブ（WooCommerce 設定内）で保存ボタンを押してください。webhook が作成されます。

請求書詳細で、webhook リクエスト送信時にエラーが出ていないかも確認してください。ホスティング事業者、ファイアウォール設定、または WordPress セキュリティプラグイン（Wordfence など）が WordPress サイトへの POST リクエストをブロックし、`403 Forbidden` や `503 Service Unavailable` になる場合があります。

サイトへのリクエストがブロックされているかは、次の 2 つの方法で確認できます。

**コマンドライン（Linux または MacOS）で確認:**
（EXAMPLE.COM を WordPress サイト URL に置き換えてください）

```
curl -vX POST -H "Content-Type: application/json" \
    -d '{"data": "test"}' https://EXAMPLE.COM/?wc-api=btcpaygf_default
```

レスポンスで「HTTP/1.1 500」または「HTTP/2 500」と「Webhook request validation failed」が表示される場合、サイトは `403 Forbidden` でリクエストをブロックしていないことを意味します。

```
.... snip ....
* We are completely uploaded and fine
< HTTP/2 500
< server: nginx
< date: Sun, 05 Jun 2022 16:55:08 GMT
< content-type: application/json; charset=UTF-8
< x-powered-by: PHP/8.1.6
< expires: Wed, 11 Jan 1984 05:00:00 GMT
< cache-control: no-cache, must-revalidate, max-age=0
<
* Connection #0 to host example.com left intact
{"code":"wp_die","message":"Webhook request validation failed.","data":{"status":500},"additional_errors":[]}
```

一方、「HTTP/1.1 403 Forbidden」または「HTTP/2 403」が表示される場合は、WordPress サイトへの送信データが何かにブロックされています。ホスティング事業者に問い合わせるか、ファイアウォールやプラグインがリクエストを遮断していないか確認してください。

```
.... snip ....
* upload completely sent off: 16 out of 16 bytes
< HTTP/1.1 403 Forbidden
< access-control-allow-origin: *
< Content-Type: application/json; charset=UTF-8
< X-Cloud-Trace-Context: 4f07d5b2e5c2f05949d04421a8e2dd6a
< Date: Thu, 17 Feb 2022 10:06:50 GMT
< Server: Google Frontend
< Content-Length: 26
```

**オンラインサービスを使って確認（コマンドラインが使えない場合）:**

- [https://reqbin.com/post-online](https://reqbin.com/post-online) にアクセス
- ドメインを入力: `https://EXAMPLE.COM/?wc-api=btcpaygf_default`
  （EXAMPLE.COM を WordPress サイト URL に置き換えてください）
- 「POST」が選択されていることを確認
- [Send] をクリック

![BTCPay WordPress V2: reqbin.com で 403 エラーをデバッグ](./img/woocommerce/btcpay-wc-2--reqbin-403-test.png)

「Status 403 (Forbidden)」が表示される場合、何らかの理由でサイトへの POST リクエストがブロックされています。ホスティング事業者に問い合わせるか、ファイアウォールやプラグインがリクエストを遮断していないか確認してください。

### チェックアウト時にエラーが出るが原因がわからない

管理ダッシュボードの BTCPay Settings（_WooCommerce -> Settings: Tab [BTCPay Settings]_）で、該当オプションのチェックボックスをオンにするとデバッグモードを有効化できます。

その後、[View Logs] ボタンをクリックするか、_WooCommerce -> Status: Tab [Logs]_ へ移動して最新の btcpay ログを選択すると、より詳細なログを確認できます。

:::warning 警告
調査が終わったら必ずデバッグモードを無効化してください。無効化しないとサイトのパフォーマンスに影響し、不要なログデータが大量にファイルシステムへ書き込まれます。
:::

加えて、BTCPay プラグイン関連エラーがないか Web サーバーのエラーログも確認してください。

### プラグイン利用時の問題やその他の質問

ヘルプが必要な場合や追加の質問がある場合は、[https://chat.btcpayserver.org/](https://chat.btcpayserver.org/) のサポートチャンネルに参加してください。

### 新しい API キーを作成する

WooCommerce V2 プラグインを 2.0.0 より前のバージョンから使っている場合、返金（pull-payment）に必要な権限が API キーに含まれていません。この機能を使いたい場合は、新しい API キーを作成できます（現時点では API キーの編集は未対応）。上記の [2.1 API キーウィザードを使って接続](#21-connect-using-the-api-key-wizard--recommended-) または [API キーの手動作成](#22-connect-by-manually-creating-the-api-key-and-permissions) を利用してください。設定済み webhook は引き続き動作するため、変更は不要です。

### webhook を触って壊してしまった場合の修復方法

誤って WooCommerce の webhook を変更し、動かなくなった場合は、BTCPay Server 側で API キーを削除したうえで、WordPress サイトの BTCPay Server Settings で再度保存を押してください。webhook が正常に再作成されたというメッセージが表示されるはずです。

<a id="deploying-woocommerce-from-btcpay-server"></a>
## BTCPay Server から WooCommerce をデプロイ

すでに BTCPay Server がある場合、既存環境から簡単に WooCommerce を開始できます。

1. BTCPay をホストしている仮想マシンの外部 IP を、ストア用ドメイン（例: store.yourdomain.com）に向けます。

2. root として BTCPay サーバーにログインします。

```bash
sudo su -
```

3. WooCommerce 用の変数を設定します。[オプション変数](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/docker-fragments/opt-add-woocommerce.yml)も追加できます。

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-add-woocommerce"
export WOOCOMMERCE_HOST="yourstoredomain.com"
```

4. 最後に BTCPay Setup スクリプトを実行して、設定した変数を反映します。

```bash
. ./btcpay-setup.sh -i
```

5. ストアのドメイン（この例では store.yourdomain.com）にアクセスし、WordPress インストールウィザードを完了してください。
