---
description: OpenCart ストアに BTCPay Server を統合する方法。
tags:
  - OpenCart
  - プラグイン
  - eコマース
---

# OpenCart で Bitcoin 決済を受け付ける

[BTCPay Server](https://btcpayserver.org/) は無料かつオープンソースの Bitcoin 決済プロセッサです。手数料や仲介業者なしで、支払いを直接あなたのウォレットで受け取れます。

OpenCart ストアを運営している場合、BTCPay Server を統合して Bitcoin を決済オプションとして提供することで、追加のマーケティングコストなしに市場へのリーチを今すぐ拡大できます。

## OpenCart で BTCPay を使うメリット

1. **完全自動化システム:** 支払い、請求書管理、返金を BTCPay が自動で処理します。

2. **手数料ゼロ:** BTCPay Server は無料です。取引手数料も処理手数料も一切かかりません。

3. **チェックアウトで Bitcoin QR コードを表示:** チェックアウト時に Bitcoin の QR コードを表示し、シンプルで安全な支払い体験を提供します。

4. **Lightning Network 統合:** 即時かつ低コストで支払いや送金を行えます。

5. **簡単なデータエクスポート:** CSV エクスポートで会計レポートを簡単に管理できます。

6. **柔軟なプラグインシステム:** ニーズに合わせて BTCPay の機能を拡張できます。

7. **POS 統合:** POS 統合により実店舗でも Bitcoin を受け付けできます。

8. **多言語対応:** 世界中の顧客に、理解しやすい言語でサービスを提供できます。

9. **セルフホスト対応インフラ:** 完全な管理のためにセルフホスト、手軽さのためにサードパーティホストを選べます。どちらでも BTCPay の全機能を利用できます。

10. **コミュニティ主導のサポート:** 専用コミュニティ（[Mattermost](http://chat.btcpayserver.org/) または [Telegram](https://t.me/btcpayserver)）で支援やアドバイスを得られます。

<a id="requirements"></a>
## 必要条件

この拡張機能をインストールする前に、以下の要件を満たしていることを確認してください。

- OpenCart 3 では PHP >= 7.4、OpenCart 4 では PHP >= 8.1
- `curl`、`gd`、`intl`、`json`、`mbstring` の PHP 拡張が利用可能
- OpenCart 3/4 ストア（[ダウンロードとインストール手順](https://www.opencart.com/index.php?route=cms/download)）
- **重要:** BTCPay Server のバージョンが 1.3.0 以降であること（[セルフホスト](/Deployment/README.md) または [サードパーティホスト](/Deployment/ThirdPartyHosting.md)）
- [インスタンスに登録済みアカウントがある](./RegisterAccount.md)
- [インスタンス上に BTCPay ストアを作成済みである](./CreateStore.md)
- [ストアにウォレットを接続済みである](./WalletSetup.md)

:::tip
この手順は OpenCart 3 をベースにしていますが、UI と操作手順は OpenCart 4 とほぼ同じです。そのため別手順は用意していません。
:::

## OpenCart 用 BTCPay 拡張機能のインストール手順
### 1. BTCPay 拡張機能をインストール

**OpenCart 用 BTCPay 拡張機能をダウンロード**する方法は 3 つあります。

- 管理ダッシュボード経由（推奨）
- [OpenCart Marketplace](https://www.opencart.com/index.php?route=marketplace/extension/info&extension_id=44269)
- [GitHub Repository](https://github.com/btcpayserver/opencart)

#### 1.1 OpenCart 管理ダッシュボードから拡張機能をインストール

注: この拡張機能は現在審査中で、まもなく利用可能になります。

#### 1.2 Marketplace または GitHub から拡張機能をダウンロードしてインストール

1. [Marketplace](https://www.opencart.com/index.php?route=marketplace/extension/info&extension_id=44269) または [Github](https://github.com/btcpayserver/opencart/releases) から最新の BTCPay 拡張機能をダウンロードします。
2. メニューで `Extensions -> Install` に移動します。
3. [Upload] ボタンをクリックし、ダウンロードした `btcpay.ocmod.zip` をアップロードします。
   アップロード完了後に "Success: You have modified extensions!" という通知が表示されます。

![BTCPay OpenCart: 拡張機能アップロード](./img/opencart/oc3--01--upload-zip.png)

#### 1.3 拡張機能のインストールを完了

1. メニューで `Extensions -> Extensions` に移動します。
2. "Choose extension type" のドロップダウンで Payment を選択します。
3. 一覧で BTCPay 拡張機能を見つけ、"Action" 列の緑色の Install ボタンをクリックします。
4. "Success: You have modified payments!" という通知が表示されます。

![BTCPay OpenCart: 拡張機能をインストール](./img/opencart/oc3--02--install-btcpay.png)

### 2. OpenCart と BTCPay Server の接続

先に進む前に、[必要条件セクション](#requirements)で説明した BTCPay Server インスタンスの準備が完了していることを確認してください。

OpenCart 向け BTCPay 拡張機能は、**BTCPay Server（決済プロセッサ）と EC ストアの橋渡し**を行います。セルフホストでもサードパーティホストでも、接続手順は同じです。

#### 2.1 OpenCart で BTCPay Server 拡張機能を設定

1. OpenCart パネルで `Extensions -> Extensions` に移動します。
2. 一覧で BTCPay 拡張機能を見つけ、青色の Edit ボタンをクリックします。
   ![BTCPay OpenCart: 新しい決済方法を追加](./img/opencart/oc3--03--configure-btcpay.png)
3. BTCPay 拡張機能を設定します。 ![BTCPay OpenCart: 決済方法の詳細](./img/opencart/oc3--04--configure-btcpay-page.png)
4. "Payment Method Enabled" フィールドを Enabled に設定します。
5. "BTCPay Server URL" フィールドに、BTCPay Server インスタンスへアクセス可能な URL（例: https://mainnet.demo.btcpayserver.org/）を入力します。BTCPay Server のデプロイ手順は[上記の必要条件セクション](#requirements)を参照してください。

続行する前に、次のセクションで説明するユーザーとストア用の API キーを作成する必要があります。すぐ戻るので、このブラウザタブは開いたままにしてください。

<a id="22-create-an-api-key-and-configure-permissions"></a>
#### 2.2 API キーを作成して権限を設定

BTCPay Server インスタンス側で以下を実施します。

1. _[Account]_ をクリック
2. _[Manage Account]_ をクリック
   ![BTCPay OpenCart: アカウント管理](./img/opencart/oc3--05--btcps-account-manage.png)
3. _"API Keys"_ タブに移動
4. _[Generate Key]_ をクリックして権限を選択
   ![BTCPay OpenCart: API Keys 概要](./img/opencart/oc3--05--btcps-account-manage-add.png)
5. "Label": ラベルを追加
6. "Permissions": **重要:** 次の権限について _"Select specific stores"_ リンクをクリックし、OpenCart サイト向けに作成したストアを選択します: `View invoices`、`Create invoice`、`Modify invoices`、`Modify stores webhooks`、`View your stores`。こうすることで API キーはそのストアのみにアクセスでき、キーが漏えいしても資金が流出しないようにできます。
   ![BTCPay OpenCart: API Keys 権限](./img/opencart/oc3--06--btcps-generate-api-key-permissions.png)
   次のような状態になります。
   ![BTCPay OpenCart: API Keys 権限](./img/opencart/oc3--07--btcps-generate-api-key-permissions-store.png)
7. 下部の _[Generate API Key]_ をクリック
8. 生成された API キーをコピーし、_OpenCart BTCPay settings_ の "BTCPay API Key" フィールドに貼り付け
   ![BTCPay OpenCart: API キーをコピー](./img/opencart/oc3--08--btcps-generate-api-key-result.png)
9. BTCPay Server インスタンスに戻り、ストア設定で Store ID をコピーして _OpenCart BTCPay Settings_ フォームに貼り付け
   ![BTCPay OpenCart: Store ID をコピー](./img/opencart/oc3--09--btcps-store-id.png)
10. _OpenCart BTCPay settings_ フォームに戻り、**BTPCay Server URL**、**API Key**、**Store ID** が設定されていることを確認して **[Save]** ボタン（右上）をクリック
    ![BTCPay OpenCart: OpenCart 設定フォームを保存](./img/opencart/oc3--10--save-settings.png)


Extensions の概要ページに戻ると、"BTCPay Server Payment details have been successfully updated." という通知が表示されます。表示されない場合は、URL、API Key、Store ID の入力内容を再確認してください。
![BTCPay OpenCart: OpenCart 設定フォームを保存](./img/opencart/oc3--11--save-settings-success.png)

保存に成功すると、BTCPay 拡張機能は支払い確定または失敗時に OpenCart へ通知する webhook を自動作成します。正常に作成されたか確認するには、BTCPay 拡張機能設定を再度開き、"Webhook Data" フィールドが次のように埋まっているか確認してください。
![BTCPay OpenCart: OpenCart 設定フォームを保存](./img/opencart/oc3--12--webhook-success.png)

BTCPay 拡張機能設定で分かるように、[請求書ステータス](https://docs.btcpayserver.org/Invoices/#invoice-statuses) などに応じて注文ステータスや一般設定をカスタマイズできます。デフォルト設定は良い出発点ですが、要件に合わせて調整してください。


### 3. チェックアウトをテストする

これで設定は完了です。テスト取引を実行しましょう。

テスト購入を行う: OpenCart ストアで少額の注文を行い、チェックアウトが期待どおりに動作することを確認します。
注文ステータスを確認する: OpenCart の注文ステータスが、対応する BTCPay の請求書ステータスに応じて更新されるか確認します。
Webhook イベントを確認する: BTCPay Server の請求書詳細で webhook イベントが成功していることを確認します。

### トラブルシューティング

#### デバッグモードを有効にする

チェックアウト中にエラーが発生した場合、BTCPay 拡張機能設定でデバッグモードを有効にできます。メニューから `Extensions -> extensions` に移動し、"Choose Extension Type" ドロップダウンで "Payments" を選択して BTCPay Server 拡張機能を編集します。

![BTCPay OpenCart: デバッグモードを有効化](./img/opencart/oc3--20--debug-mode-enable.png)

デバッグ出力は、メニュー `System -> Maintenence -> Error Logs` の `error log` で確認できます。

![BTCPay OpenCart: デバッグモードを有効化](./img/opencart/oc3--21--error-logs.png)

\*デバッグが終わったら必ず無効化してください。無効化しないとエラーログが埋まってしまいます。\*\*

**エラー例**:

> 2022-05-24 21:10:50 ERROR Error during POST to https://btcpay.example.com/api/v1/stores/4kD5bvAF5j8DokHqAzxb6MFDV4ikabcdefghijklm/invoices. Got response (401): {&quot;code&quot;:&quot;unauthenticated&quot;,&quot;message&quot;:&quot;Authentication is required for accessing this endpoint&quot;}

- これは認証エラーがあることを意味します。多くの場合、API キーにそのストアで請求書を作成する権限がありません。API キーに正しい権限を付与し、正しいストアを選択し、その値を OpenCart の決済設定フォームに入力してください。

- もう 1 つの理由として、レガシー API キーを使用している可能性があります。レガシー API キーは `store settings -> Access Tokens` にありますが、必要なのは `Account -> Manage Account -> "API Keys"` タブで作成するアカウント API キーです。[2.2 API キーを作成して権限を設定](#22-create-an-api-key-and-configure-permissions)を参照してください。

### 請求書は支払い済みなのに注文ステータスが更新されない

請求書詳細で webhook リクエスト送信時のエラーがないか確認してください。ホスティング事業者、ファイアウォール設定、またはセキュリティ拡張機能により、サイトへの POST リクエストがブロックされ、HTTP ステータスが "403 forbidden" になることがあります。

サイトへのリクエストをブロックする要因があるかどうかは、次の 2 つの方法で確認できます。

**1. webhook コールバック URL をコピーする**  
_OpenCart BTCPay extension settings_ に移動し、"Webhook Data" フィールドの "URL" をコピーします。例: `https://YOURSTOREDOMAIN.TLD/index.php?route=extension/payment/btcpay/callback`

![BTCPay OpenCart: OpenCart 設定フォームを保存](./img/opencart/oc3--12--webhook-success.png)

**2.1 コマンドラインで確認する（Linux または MacOS）:**

```
curl -vX POST -H "Content-Type: application/json" \
    -d '{"data": "test"}' WEBHOOK_CALLBACK_URL
```

（`WEBHOOK_CALLBACK_URL` は上でコピーしたものに置き換えてください）

結果:

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

"HTTP/1.1 403 Forbidden" または "HTTP/2 403" が表示される場合、OpenCart サイトへのデータ送信が何かにブロックされています。ホスティング事業者に問い合わせるか、ファイアウォールやセキュリティ拡張がリクエストを遮断していないか確認してください。

**2.2 オンラインサービスで確認する（コマンドラインが使えない場合）:**

- [https://reqbin.com/post-online](https://reqbin.com/post-online) にアクセス
- 1. コールバック URL（上記手順 1 でコピー）を入力: `https://YOURSTOREDOMAIN.TLD/index.php?route=extension/payment/btcpay/callback`
     （この URL を手順 1 の webhook コールバック URL に置き換えてください）
- "POST" が選択されていることを確認
- 2. [Send] をクリック

![BTCPay OpenCart: Webhook payload URL forbidden](./img/virtuemart/btcpay-vm--19-troubleshoot-403-callback.png)

"**Status 403 (Forbidden)**" と表示される場合、何らかの理由でサイトへの POST リクエストがブロックされています。ホスティング事業者に確認するか、ファイアウォールやセキュリティ拡張がリクエストを遮断していないことを確認してください。別のステータスコード（200、500、...）が表示される場合、ファイアウォール問題ではない可能性が高く、追加調査が必要です。

## 拡張機能の利用で問題がある、または関連する質問がある場合

サポートが必要な場合や追加の質問がある場合は、[https://chat.btcpayserver.org/](https://chat.btcpayserver.org/) または [https://t.me/btcpayserver](https://t.me/btcpayserver) のサポートチャネルへ参加してください。
