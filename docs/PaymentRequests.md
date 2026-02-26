# 支払いリクエスト (Payment Requests)

Payment Requests は、BTCPay のストアオーナーが長期間有効な請求書を作成できる機能です。
Payment Request への支払いは、支払い時点の為替レートを使用します。
これにより、ユーザーは支払い時にストアオーナーと為替レートを交渉または確認することなく、都合のよいタイミングで支払えます。

ユーザーはリクエストに対して分割支払いできます。
Payment Request は、全額支払い完了まで、またはストアオーナーが有効期限を設定した場合はその期限まで有効です。
アドレスは再利用されません。ユーザーが支払いをクリックして Payment Request 用請求書を作成するたびに、新しいアドレスが生成されます。

ストアオーナーは、記録保管や会計のために Payment Request を印刷（または請求書データをエクスポート）することもできます。
BTCPay はストアの請求書一覧で、該当請求書に自動的に Payment Requests ラベルを付与します。

## 支払いリクエスト動画

[![BTCPay Server の Payment Requests](https://img.youtube.com/vi/j6CvwDPvfzQ/mqdefault.jpg)](https://www.youtube.com/watch?v=j6CvwDPvfzQ)


## Payment Request の作成

`Requests > Create Requests` をクリックします。

![Payment Request を作成](./img/payment-requests/PaymentRequestList.png)


![Payment Request 作成画面](./img/payment-requests/CreatePaymentRequest.png)


Payment Request 作成時には、次の詳細を入力します:

- **Title**: Payment Request のタイトル
- **Amount & Currency**: 法定通貨または暗号通貨での請求金額
- **Expiration Time**: 支払いを有効とする期限日（任意）
- **Email**: 指定した場合、このリクエストへの支払いが行われるたびに通知を受け取るメールアドレス
- **Request Customer data on checkout**: メールアドレスや配送先住所など、顧客情報の入力を求められます
- Memo: 顧客向けのメモを残したい場合は、memo セクションに記載できます。メッセージの書式設定や添付ファイル追加が可能なテキストエディターを利用できます


分割支払いを許可したい場合は、_Allow payee to create invoices in their own denomination_ を選択してください。

:::warning
Payment Request はストア依存です。つまり、各 Payment Request は作成時に 1 つのストアに紐付きます。
Payment Request が属するストアに、必ずウォレットを接続してください。
:::

`Create` をクリックして Payment Request を確認します。

![新しい Payment Request を表示](./img/payment-requests/NewPaymentRequest.png)

BTCPay は Payment Request 用の URL を作成します。この URL を共有すると、Payment Request を表示できます。
同じリクエストを複数作りたいですか？ メインメニューの `Clone` オプションで、以下のように Payment Request を複製できます。


![Payment Request 一覧を表示](./img/payment-requests/PaymentRequestListOptions.png)

## 支払い済み Payment Request

支払い送信後、受取人と依頼者の双方が Payment Request の状態を確認できます。
支払いが全額受領されると、状態は **Settled** と表示されます。
一部のみ支払われた場合は、Amount Due に残額が表示されます。

![支払い済み Payment Request を表示](./img/payment-requests/PaidPaymentRequest.png)
