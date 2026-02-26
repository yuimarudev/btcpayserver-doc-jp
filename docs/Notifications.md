# 通知

BTCPay Server のイベントを監視するための通知は、いくつかの方法で設定できます。

- [通知アラート](#notification-alerts)
- [サーバーレベル SMTP (Email)](#server-emails)
- [ストアレベル SMTP (Email)](#store-emails)
- [ストアレベル Webhook](https://docs.btcpayserver.org/API/Greenfield/v1/#tag/Webhooks)

<a id="notification-alerts"></a>
## 通知アラート

現在の通知を確認するには、メインヘッダーの通知アイコンをクリックします。通知ページには、請求書イベント、払い出し、バージョン更新など、現在の通知の状態が表示されます。通知は、通知ドロップダウンまたは通知ページから既読にできます。

![BTCPay 通知](./img/notifications/notification-page.png)

サーバーに登録されている各ユーザーは、受信する通知を管理できます。

![BTCPay 通知管理](./img/notifications/notification-manage.png)

<a id="server-emails"></a>
## サーバーメール

メールは BTCPay のサーバーレベルから送信できます。これらは [ユーザーメール](#user-emails) です。管理者は次の場所でサーバー SMTP を設定できます:

Server Settings > Email server > [Setup](#smtp-email-setup)

<a id="store-emails"></a>
## ストアメール

メールは BTCPay のストアレベルから送信できます。これらのメールは請求書など、ストア関連イベント向けです。ユーザーは次の場所でストア SMTP を設定できます:

Store Settings > General Settings > Services > Email > [Setup](#smtp-email-setup)

<a id="smtp-email-setup"></a>
### SMTP メール設定

よく使うメールクライアント設定値は、Quick fill settings ドロップダウンで入力できます。同じページからテストメールを自分宛に送って、設定が正しく動作することを確認してください。

![BTCPay Email SMTP](./img/smtp/smtp-setup.png)

![BTCPay Email SMTP](./img/smtp/validate-smtp-setup.png)

SMTP の設定要件はメールクライアントごとに異なる場合があります。詳しくはこの [SMTP FAQ](./FAQ/ServerSettings.md#how-to-configure-smtp-settings-in-btcpay) を参照するか、メールプロバイダーのドキュメントを確認してください。

<a id="user-emails"></a>
# ユーザーメール

BTCPay Server には、ユーザーとのやり取りに利用できるさまざまなユーザーメール機能が組み込まれています。

:::warning
ユーザーメールは、サーバーで SMTP が有効な場合にのみ送信されます。
:::

- [パスワード忘れ](#forgot-password-email)
- [新規ユーザー確認](#new-user-confirmation-email)
- [新規ユーザー招待](#new-user-invitation-email)
- [カスタムメール](#custom-emails)

<a id="forgot-password-email"></a>
## パスワード忘れメール

このメールは、パスワードを失念したユーザーに送信できます。サーバーで SMTP が有効でない場合、サーバー管理者のパスワードを含むすべてのユーザーパスワードをリセットする [簡単な方法](./FAQ/ServerSettings.md#forgot-btcpay-admin-password) はありません。パスワードは安全な場所に保存するか、サーバーにメール設定を行ってください。

<a id="new-user-confirmation-email"></a>
## 新規ユーザー確認メール

このメールは、新規ユーザーアカウント登録を確認するために使用されます。スパム登録を減らすために、サーバー管理者がメール確認を必須にする場合があります（サーバー設定ポリシーで設定）。新規ユーザーはこのメール内のリンクをクリックしてアカウントを検証し、登録プロセスを完了できます。

<a id="new-user-invitation-email"></a>
## 新規ユーザー招待メール

招待メールを送信して、サーバー上でアカウント登録する [新規ユーザーを招待](./FAQ/ServerSettings.md#how-to-add-a-new-user-by-invite) できます。これにより、サーバー登録を一般公開せずに新規ユーザーだけを招待できます。

<a id="custom-emails"></a>
## メールルール

メールルールを使うと、イベントに応じてストアから送信するメールを BTCPay Server でカスタマイズできます。
`Configure` ボタンをクリックし、新しい Email rule を `create` してください。

次の中から Email trigger を設定します:

- 請求書作成 (`Invoice created`)
- 請求書の支払い受領 (`Invoice Received Payment`)
- 請求書処理中 (`Invoice Processing`)
- 請求書期限切れ (`Invoice Expired`)
- 請求書決済完了 (`Invoice Settled`)
- 請求書無効 (`Invoice Invalid`)
- 請求書支払い決済完了 (`Invoice Payment Settled`)

ストアのイベント通知を受け取りたい受信先メールアドレスを設定するか、`Send email to the buyer if an email was provided to the invoice` オプションにチェックを入れてください。
イベントメールの件名を設定し、最後に本文を任意にスタイル設定できます。

現在利用できるプレースホルダーは次のとおりです:

```
            {Invoice.Id}
            {Invoice.StoreId}
            {Invoice.Price}
            {Invoice.Currency}
            {Invoice.Status}
            {Invoice.OrderId}
```

更新の可能性を含むソースは [こちら](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer/HostedServices/StoreEmailRuleProcessorSender.cs) で確認できます。

![新しい Email rule を作成](./img/FAQ/btcpayemailrule1.jpg)
