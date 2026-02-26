---
description: BTCPay Server を Joomla VirtueMart ストアに統合する方法
tags:
  - VirtueMart
  - Joomla
  - プラグイン
  - eコマース
---

# Joomla VirtueMart 統合

このドキュメントでは、**BTCPay Server を Joomla VirtueMart ストアに統合する方法**を説明します。  
ドキュメントに沿って進めるために、以下の動画もご覧ください。

[![BTCPay Server - Joomla VirtueMart](https://img.youtube.com/vi/k7XfybLAky0/mqdefault.jpg)](https://youtu.be/k7XfybLAky0)

## 要件

このプラグインをインストールする前に、以下の要件を満たしていることを確認してください。

- PHP バージョン 7.4 以上
- PHP 拡張の `curl`、`gd`、`intl`、`json`、`mbstring` が利用可能
- VirtueMart 3 / 4 ストア（[ダウンロードとインストール手順](https://www.virtuemart.net/downloads)）
- BTCPay Server 1.3.0 以降を利用している（[セルフホスト](/Deployment/README.md) または [サードパーティホスティング](/Deployment/ThirdPartyHosting.md)）
- [インスタンス上に登録済みアカウントがある](./RegisterAccount.md)
- [インスタンス上に BTCPay ストアがある](./CreateStore.md)
- [ストアにウォレットが接続されている](./WalletSetup.md)

## 1. BTCPay プラグインをインストールする

**VirtueMart 用 BTCPay プラグイン**のダウンロード方法は 3 つあります。

- 管理ダッシュボード経由（推奨、以下参照）
- [Joomla Extension Directory (JED)](https://extensions.joomla.org/extension/vm-payment-btcpay-for-virtuemart/)
- [GitHub リポジトリ](https://github.com/btcpayserver/joomla-virtuemart/releases)

### 1.1 Joomla 管理ダッシュボードからプラグインをインストールする（推奨）

1. メニュー: Extensions > Manage > Install
2. 「Install from Web」タブで「btcpay」を検索
3. BTCPay for VirtueMart をクリックし、[Install] ボタンを押す
4. 手順 1.3 に進む

![BTCPay Virtuemart: ウェブからのプラグインインストール](./img/virtuemart/btcpay-vm--01-install-web.png)

### 1.2 JED または GitHub からプラグインをダウンロードしてインストールする

1. 最新の BTCPay プラグインを [Github](https://github.com/btcpayserver/joomla-virtuemart/releases) または [JED](https://extensions.joomla.org/extension/vm-payment-btcpay-for-virtuemart/) からダウンロード
2. メニュー: Extensions -> Manage -> Install
3. 「Upload Package File」タブで `btcpayvm.zip` をアップロード

![BTCPay Virtuemart: アップロードでのプラグインインストール](./img/virtuemart/btcpay-vm--02-install-upload.png)

### 1.3 プラグインを有効化する

1. メニュー: Extensions -> Plugins
2. 「btcpay」を検索
3. 「Status」列の赤い丸をクリックしてプラグインを有効化

![BTCPay Virtuemart: プラグインを有効化](./img/virtuemart/btcpay-vm--03-enable-plugin.png)

## 2. VirtueMart と BTCPay Server を接続する

BTCPay for Virtuemart プラグインは、**BTCPay Server（決済処理）と EC ストアをつなぐブリッジ**です。  
セルフホストでもサードパーティ運用でも、接続手順は同じです。

### 2.1 VirtueMart に BTCPay 決済ゲートウェイを追加する

1. メニュー: VirtueMart -> Payment Methods
2. **[New]** ボタンをクリック  
   ![BTCPay Virtuemart: 新しい決済方法を追加](./img/virtuemart/btcpay-vm--04-add-new-payment-method.png)
3. 必要に応じて決済方法を設定します。「Payment Method」ドロップダウンで「BTCPay for VirtueMart」が選択され、決済方法が公開状態であることを確認してください。 ![BTCPay Virtuemart: 決済方法の詳細](./img/virtuemart/btcpay-vm--05-payment-method-details.png)
4. **[Save]** ボタンをクリック（プラグイン用テーブルが作成されます）

これで「Configuration」タブに切り替え、BTCPay Server インスタンスへ接続できます。まず API キーを作成します。

![BTCPay Virtuemart: 決済方法の設定タブ](./img/virtuemart/btcpay-vm--06-payment-method-configuration-tab.png)

### 2.2 API キーを作成して権限を設定する
<a id="22-create-an-api-key-and-configure-permissions"></a>

BTCPay Server インスタンス側で:

1. _[Account]_ をクリック
2. _[Manage Account]_ をクリック  
   ![BTCPay Joomla VirtueMart: アカウント管理](./img/virtuemart/btcpay-vm--07-account-manage.png)
3. _"API Keys"_ タブへ移動
4. _[Generate Key]_ をクリックして権限を選択  
   ![BTCPay Joomla VirtueMart: API Keys 概要](./img/virtuemart/btcpay-vm--08-add-api-key.png)
5. ラベルを追加します。**重要:** 次の権限について _"Select specific stores"_ リンクをクリックしてください: `View invoices`、`Create invoice`、`Modify invoices`、`Modify stores webhooks`、`View your stores`。その後、VirtueMart サイト用に作成したストアを選択します。すべて正しく設定できると次のようになります。  
   ![BTCPay Joomla VirtueMart: API Keys の権限](./img/virtuemart/btcpay-vm--09-permissions-and-select-store.png)
6. _[Generate API Key]_ をクリック  
   ![BTCPay Joomla VirtueMart: API Keys 保存](./img/virtuemart/btcpay-vm--10-permissions-set.png)
7. 生成された API Key を _VirtueMart BTCPay Payment Method Settings_ フォームへコピー  
   ![BTCPay Joomla VirtueMart: API Key をコピー](./img/virtuemart/btcpay-vm--11-copy-api-key.png)
8. Settings に移動し、Store ID を _VirtueMart BTCPay Payment Method Settings_ フォームへコピー  
   ![BTCPay Joomla VirtueMart: Store ID をコピー](./img/virtuemart/btcpay-vm--12-copy-store-id.png)
9. _VirtueMart BTCPay Payment Method Settings_ フォームで **BTPCay Server URL**、**API Key**、**Store ID** が設定されていることを確認し、**[Save]** をクリック  
   ![BTCPay Joomla VirtueMart: VirtueMart 設定フォームを保存](./img/virtuemart/btcpay-vm--13-save-vm-payment-method-form.png)

### 2.3 BTCPay Server に webhook を作成する
<a id="23-create-a-webhook-on-btcpay-server"></a>

webhook を設定すると、BTCPay Server から請求書ステータス変更の更新を受け取れるため重要です。

1. BTCPay Server インスタンスでストア設定の **[Webhooks]** タブへ移動し、**[Create Webhook]** をクリック  
   ![BTCPay Joomla VirtueMart: webhook を作成](./img/virtuemart/btcpay-vm--14-create-webhook.png)
2. _VirtueMart BTCPay Payment Method Settings_ から **Webhook callback URL** をコピーし、webhook 設定の **Payload URL** に貼り付けます。  
   ![BTCPay Joomla VirtueMart: Webhook payload URL](./img/virtuemart/btcpay-vm--15-webhook-payload-url.png)
3. webhook 設定で目のアイコンをクリックして webhook secret を表示し、その secret を _VirtueMart BTCPay Payment Method Settings_ フォームの **Webhook Secret** 入力欄へコピーして、VirtueMart 設定を再度 **[Save]** します。  
   ![BTCPay Joomla VirtueMart: Webhook payload URL](./img/virtuemart/btcpay-vm--16-webhook-copy-secret.png)
   ![BTCPay Joomla VirtueMart: Webhook VM 設定を保存](./img/virtuemart/btcpay-vm--16-virtuemart-configuration-save.png)

4. webhook 設定画面に戻り、**Automatic redelivery** を有効にして **[Add webhook]** をクリックし保存します。  
   ![BTCPay Joomla VirtueMart: Webhook payload URL](./img/virtuemart/btcpay-vm--17-webhook-save.png)

## 3. チェックアウトをテストする

これで準備完了です。少額のテスト購入を行い、注文ステータスが BTCPay 請求書ステータスに応じて更新されることを確認してください。BTCPay Server の請求書詳細では、webhook イベントが正常に発火したか確認できます。

## VirtueMart BTCPay 決済方法設定のカスタマイズ

VirtueMart の BTCPay 決済方法設定は、メニュー `VirtueMart -> Payment Methods` から確認できます。作成した「btcpayvm」タイプの決済方法をクリックしてください。

### セクション: BTCPay Server 接続設定

ここは設定の中で最も重要な部分です。ここに入力するデータにより、VirtueMart ショップと BTCPay Server 側の対応ストアが接続されます。

**BTCPay Server URL**

BTCPay Server インスタンスの URL（プロトコルを含む）。例: `https://btcpay.yourdomain.com`。

**API Key**

[こちら](#22-create-an-api-key-and-configure-permissions)で説明した BTCPay API Key。

**Store ID**

BTCPay Server ストアのストア ID。ストア設定ページで確認できます。[こちら](#22-create-an-api-key-and-configure-permissions)の 8 を参照してください。

**Webhook Secret**

webhook 作成時に生成された secret。[こちら](#23-create-a-webhook-on-btcpay-server)を参照。

**Webhook callback URL**

このフィールドはプラグインが自動生成します。BTCPay Server で webhook を作成する際に使用します。必要な決済方法 ID とパラメータを含み、コールバック処理を可能にします。

### セクション: 注文ステータスのマッピング

BTCPay Server の請求書ステータスと VirtueMart の注文ステータスの対応を調整できます。左側が請求書ステータス、右側が注文ステータスです。通常はデフォルト設定で問題ありませんが、必要に応じて上書きできます。

VirtueMart の注文ステータスの説明は[こちら](https://docs.virtuemart.net/manual/configuration-menu/order-statuses.html)

BTCPay Server の請求書ステータスの説明は[こちら](https://docs.btcpayserver.org/Invoices/#invoice-statuses)

### セクション: 制限

これらは VirtueMart が提供する、他の決済プラグインでも使われる制限設定です。決済方法を利用可能にする金額や国を制限できます。

### セクション: 割引と手数料

これらは VirtueMart が提供する設定です。手数料、キャッシュバック、税ルールの適用、または決済方法のカスタムロゴ設定が可能です。

## トラブルシューティング

### チェックアウト時のエラー「There was an error processing the payment on BTCPay Server. Please try again and contact us if the problem persists.」

これは BTCPay Server での請求書作成中に何らかの問題が発生したことを意味します。API キー、Store ID の誤り、または通信エラーの可能性があります。プラグインのエラーログは `administrator/logs` ディレクトリにあります。`btcpayvm.X.log.php`（`X` は番号、例: `btcpayvm.0.log.php`）というファイルが 1 つ以上あり、時刻付きのエラーから原因の手がかりを得られます。

**例**:

> 2022-05-24 21:10:50 ERROR Error during POST to https://btcpay.example.com/api/v1/stores/4kD5bvAF5j8DokHqAzxb6MFDV4ikabcdefghijklm/invoices. Got response (401): {&quot;code&quot;:&quot;unauthenticated&quot;,&quot;message&quot;:&quot;Authentication is required for accessing this endpoint&quot;}

- これは認証エラーです。多くの場合、API キーにそのストアで請求書を作成する権限がありません。API キーに正しい権限を付与し、正しいストアに紐付け、VirtueMart の決済設定フォームにも正しく入力されていることを確認してください。

- 別の原因として、レガシー API キーを使っている可能性があります。レガシー API キーは `store settings -> Access Tokens` にありますが、必要なのは `Account -> Manage Account -> tab "API Keys"` にあるアカウント API キーです。セクション [2.2 API キーを作成して権限を設定する](#22-create-an-api-key-and-configure-permissions) を参照してください。

## 請求書が支払い済みなのに注文ステータスが更新されない

請求書詳細で webhook リクエスト送信時のエラーを確認してください。ホスティング事業者、ファイアウォール設定、または Joomla セキュリティプラグインにより、サイトへの POST リクエストがブロックされ、HTTP ステータス `403 forbidden` になることがあります。

以下の 2 つの方法で、サイトへのリクエストがブロックされているか確認できます。

**1. webhook callback URL をコピー**  
_VirtueMart BTCPay Payment Method Settings_ へ移動し、「Webhook callback URL」をコピーします。例: `https://EXAMPLE.COM/index.php?option=com_virtuemart&view=pluginresponse&task=pluginnotification&pm=2`

![BTCPay Joomla VirtueMart: Webhook payload URL](./img/virtuemart/btcpay-vm--18-troubleshoot-copy-callback-url.png)

**2.1 コマンドライン（Linux または MacOS）で確認:**

```
curl -vX POST -H "Content-Type: application/json" \
    -d '{"data": "test"}' WEBHOOK_CALLBACK_URL
```

（`WEBHOOK_CALLBACK_URL` を上でコピーした URL に置き換えてください）

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

`HTTP/1.1 403 Forbidden` または `HTTP/2 403` が表示される場合、VirtueMart サイトへのデータ送信を何かがブロックしています。ホスティング事業者に確認するか、ファイアウォールやプラグインがリクエストをブロックしていないか確認してください。

**2.2 オンラインサービスで確認（コマンドラインが使えない場合）:**

- [https://reqbin.com/post-online](https://reqbin.com/post-online) にアクセス
- 1. callback URL（手順 1 でコピー）を入力: `https://EXAMPLE.COM/index.php?option=com_virtuemart&view=pluginresponse&task=pluginnotification&pm=2`  
     （この URL を手順 1 の webhook callback URL に置き換えてください）
- 「POST」が選択されていることを確認
- 2. [Send] をクリック

![BTCPay Joomla VirtueMart: Webhook payload URL forbidden](./img/virtuemart/btcpay-vm--19-troubleshoot-403-callback.png)

**Status 403 (Forbidden)** と表示される場合、何らかの理由でサイトへの POST リクエストがブロックされています。ホスティング事業者に確認するか、ファイアウォールやプラグインがブロックしていないか確認してください。別のステータスコード（200、500 など）の場合、ファイアウォール問題ではない可能性が高く、さらに調査が必要です。

## プラグインの利用で問題がある、またはその他の関連質問がある場合

サポートや追加の質問が必要な場合は、[https://chat.btcpayserver.org/](https://chat.btcpayserver.org/) のサポートチャネルに参加してください。
