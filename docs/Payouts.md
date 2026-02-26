# 払い出し (Payouts)

払い出し機能は [Pull Payments](./PullPayments.md) と連動しています。この機能を使うと、BTCPay 内で払い出しを作成できます。
この機能により、pull payment（返金、給与払い出し、出金）を処理できます。

## どのように動作しますか？

ここでは 2 つの例を見ていきます。1 つは返金、もう 1 つは給与払い出しです。

### 例

まず返金の例から始めます。
顧客があなたのストアで商品を購入したものの、残念ながら返品することになり、返金を希望しているとします。
BTCPay では [Refund](./Refund.md) を作成し、顧客に資金受け取り用リンクを提供できます。
顧客がアドレスを入力して資金を請求すると、その内容が `Payouts` に表示されます。

最初のステータスは `Awaiting Approval` です。
ストア担当者は待機中の項目を確認し、選択後に `Actions` ボタンを使用します。

`Actions` ボタンのオプション

- 選択した払い出しを承認 (`Approve selected payouts`)
- 選択した払い出しを承認して送信 (`Approve & send selected payouts`)
- 選択した払い出しをキャンセル (`Cancel selected payouts`)

返金したいので、次の手順は `Approve & send selected payouts` です。
顧客アドレス、金額、手数料を返金額から差し引くかどうかを確認します。
確認が終われば、残る作業はトランザクションへの署名だけです。

顧客側の Claiming ページも更新されます。ブロックエクスプローラーへのリンクと取引情報が表示されるため、トランザクションを追跡できます。
トランザクションが承認されると、ステータスは Completed に変わります。

次に給与払い出しの例です。これは顧客リクエスト起点ではなく、ストア内部から開始します。
基本は同じで Pull payments を利用します。ただし返金を作成する代わりに [Pull Payment](./PullPayments.md) を作成します。

BTCPay Server の `Pull Payments` タブに移動します。
右上の `Create Pull Payment` ボタンをクリックします。

払い出し作成画面で、名前、希望金額、希望通貨を入力し、説明を記入します。これで従業員は内容を把握できます。
次の部分は返金と同様です。従業員は送金先アドレスと、この払い出しから請求したい金額を入力します。2 回に分けて別アドレスに請求したり、一部を Lightning で請求したりすることもできます。

待機中の Payouts が複数ある場合は、まとめて署名・送信できます。署名後、払い出しは `In progress` タブに移動し、トランザクションが表示されます。
ネットワークに受理されると、払い出しは Completed タブへ移動します。
Completed タブは履歴目的です。処理済み Payouts と、それに対応するトランザクションが保存されます。

![BTCPay Server の Payouts タブ](./img/refunds/batch-payouts.jpg)

## Greenfield API の利用

[Pull Payments](./PullPayments.md#greenfield-api) で説明されているように、Greenfield API を使うと `Pull Payments` をより広く活用できます。
この仕組みを使うと、払い出しリクエストは常に BTCPay Server の Payouts タブに表示されます。
Greenfield API を使えば、これらのリクエストを自動化できます。将来の BTCPay Server リリースでは、払い出し自動化オプションが追加される可能性があります。
