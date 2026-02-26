# cURL を使った Greenfield API の例

**[Greenfield API](https://docs.btcpayserver.org/API/Greenfield/v1/)**（`/docs` としてあなたのインスタンスでも利用可能）を使うと、使いやすい REST API 経由で BTCPay Server を操作できます。

[Swagger file](https://docs.btcpayserver.org/API/Greenfield/v1/swagger.json) を使えば、任意の言語向けクライアントを部分的に生成できる点にも注意してください。

このガイドでは、Linux のコマンドラインで `curl` と `jq` を使う方法を紹介します。

## 前提条件

特定ユーザーの代理でストアや API キーを作成するような一部のエンドポイントを除き、Basic Auth は避け、代わりに API キーを使うべきです。API キーには必要最小限の権限だけを付与してください。たとえば請求書作成だけを行うなら、ストア管理権限は不要です。

BTCPay Server UI の `Account` -> `Manage account` -> `API keys` で新しい API キーを作成できます。

以下の eCommerce 例では、API キーに次の権限が必要です。
- View invoices
- Create invoice
- Modify invoices
- Modify stores webhooks
- View your stores
- 未承認の Pull Payment を作成

利用可能な権限の一覧は [API documentation](https://docs.btcpayserver.org/API/Greenfield/v1/#section/Authentication/API_Key)、または各エンドポイントに記載された権限情報を参照してください。

## eCommerce の例

以下の例では、請求書の作成、Webhook の登録、Webhook の処理、請求書の全額返金という流れで、Greenfield API を使った基本的な eCommerce フローを示します。

### 請求書を作成する

[create invoice endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_CreateInvoice) を使って請求書を作成します。これはシンプルな例ですが、注文 ID、購入者メール、カスタムメタデータなど多くの情報を設定できます。ただし、侵害時の情報漏えいを防ぐため、請求書には冗長なデータを保存しないでください。たとえば多くの場合、顧客住所を eCommerce システムと BTCPay 請求書の両方に保存する意味はありません。

```bash
BTCPAY_INSTANCE="https://mainnet.demo.btcpayserver.org"
API_KEY="YOUR_API_KEY"
STORE_ID="YOUR_STORE_ID"
AMOUNT="10"
CURRENCY="USD"

BODY="$(echo "{}" | jq --arg "a" "$AMOUNT" '. + {amount:$a}' \
                  | jq --arg "a" "$CURRENCY" '. + {currency:$a}')"

curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token $API_KEY" \
     -X POST \
     -d "$BODY" \
     "$BTCPAY_INSTANCE/api/v1/stores/$STORE_ID/invoices"
```

### Webhook を登録する（任意）

請求書が支払われたときに通知を受けるため、Webhook を登録します。登録には [create webhook endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Webhooks_CreateWebhook) を使用できます。

```bash
BTCPAY_INSTANCE="https://mainnet.demo.btcpayserver.org"
API_KEY="YOUR_API_KEY"
STORE_ID="YOUR_STORE_ID"

URL="https://example.com/your-webhook-endpoint"

BODY="$(echo "{}" | jq --arg "a" "$URL" '. + {url:$a}')"

curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token $API_KEY" \
     -X POST \
     -d "$BODY" \
     "$BTCPAY_INSTANCE/api/v1/stores/$STORE_ID/webhooks"
```

この手順は任意です。ストアの `Settings` -> `Webhooks` から BTCPay Server UI 上で手動作成することもできます。

### Webhook を検証して処理する

これは bash の curl だけでは難しく、Web サーバー上で実装します。例は [NodeJS](./GreenFieldExample-NodeJS.md) と [PHP](./GreenfieldExample-PHP.md) を参照してください。

### 請求書を全額返金する

[invoice refund endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Invoices_Refund) を使うと、請求書の全額（または一部）返金を実行できます。返金を顧客が請求できるリンクが返されます。

```bash
BTCPAY_INSTANCE="https://mainnet.demo.btcpayserver.org"
API_KEY="YOUR_API_KEY"
STORE_ID="YOUR_STORE_ID"

INVOICE_ID="EXISTING_INVOICE_ID"
PAYMENT_METHOD="BTC"
REFUND_VARIANT="CurrentRate"

BODY="$(echo "{}" | jq --arg "a" "$REFUND_VARIANT" '. + {refundVariant:$a}' \
                  | jq --arg "a" "$PAYMENT_METHOD" '. + {paymentMethod:$a}')"

curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token $API_KEY" \
     -X POST \
     -d "$BODY" \
     "$BTCPAY_INSTANCE/api/v1/stores/$STORE_ID/invoices/$INVOICE_ID/refund"
```

## BTCPay Server 管理の例

ここでは、あなたがアンバサダーとしてユーザー向けに BTCPay Server をホストしている想定です。ユーザー管理は自分のシステムで行い、BTCPay Server ログイン用のメールとパスワードを設定してユーザーを作成します。その後、同じ認証情報を使ってユーザーの代理でストアと API キーを作成します。

### 新しいユーザーを作成する

新規ユーザーは [this endpoint](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Users_CreateUser) で作成できます。

```bash
BTCPAY_INSTANCE="https://mainnet.demo.btcpayserver.org"
ADMIN_API_KEY="YOUR_ADMIN_API_KEY"

USER="satoshi.nakamoto@example.com"
PASSWORD="SuperSecurePasswordsShouldBeQuiteLong123"

BODY="$(echo "{}" | jq --arg "a" "$USER" '. + {email:$a}' \
                  | jq --arg "a" "$PASSWORD" '. + {password:$a}')"
curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token $ADMIN_API_KEY" \
     -X POST \
     -d "$BODY" \
     "$BTCPAY_INSTANCE/api/v1/users"
```

### ユーザーの代理でストアを作成する

新規ユーザーの認証情報を使ってストアを作成します。ユーザーがオーナーになります。[create a new store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_CreateStore)

```bash
STORE_NAME="My awesome store"

BODY="$(echo "{}" | jq --arg "a" "$STORE_NAME" '. + {name:$a}')"

NEW_STORE_ID="$(curl -s \
     -H "Content-Type: application/json" \
     --user "$USER:$PASSWORD" \
     -X POST \
     -d "$BODY" \
     "$BTCPAY_INSTANCE/api/v1/stores"  | jq -r .id)"

echo "New store id: $NEW_STORE_ID"
```

### ユーザーの代理で新しい API キーを作成する

これで API キーを作成し、たとえば `btcpay.store.canmodifystoresettings` 権限で新しいストアに限定できます。通常は請求書作成権限も許可したいはずですが、この例では簡略化しています。

エンドポイントに必要な権限は、各エンドポイントドキュメントの "Authorization" か、[authorization section](https://docs.btcpayserver.org/API/Greenfield/v1/#section/Authentication/API_Key) の権限一覧で確認できます。

```bash
ADMIN_API_KEY="YOUR_ADMIN_API_KEY"
USER="satoshi.nakamoto@example.com"
PERMISSION="btcpay.store.canmodifystoresettings"
NEW_STORE_ID="NEW_STORE_ID_FROM_PREVIOUS_STEP"

BODY="$(echo "{}" | jq --arg "a" "$PERMISSION:$NEW_STORE_ID" '. + {permissions:[$a]}')"
USER_API_KEY="$(curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token $ADMIN_API_KEY" \
     -X POST \
     -d "$BODY" \
     "$BTCPAY_INSTANCE/api/v1/users/$USER/api-keys"  | jq -r .apiKey)"

echo "New user api key: $USER_API_KEY"
```

### ストア情報を読み取る

新しい API キーを使って [read store](https://docs.btcpayserver.org/API/Greenfield/v1/#operation/Stores_GetStore) 情報を取得できます。

```bash
USER_API_KEY="API_KEY_FROM_PREVIOUS_STEP"
NEW_STORE_ID="NEW_STORE_ID_FROM_BEFORE_PREVIOUS_STEP"

curl -s \
     -H "Content-Type: application/json" \
     -H "Authorization: token $USER_API_KEY" \
     -X GET \
     "$BTCPAY_INSTANCE/api/v1/stores/$NEW_STORE_ID"
```
