# フォーム

BTCPay Server のフォームビルダーを使うと、顧客から特定の情報を収集できます。

これらのフォームは要件に合わせて完全にカスタマイズできます。
この入門ではビジュアルフォームビルダーを扱います。より高度な設定を行いたい場合は、ドキュメントの [Advanced Forms](./AdvancedForms.md) セクションをご覧ください。

## ストア最初のカスタムフォームを設定する

この例では、あらかじめ用意されている標準フォームを作成します。
`Store Settings` をクリックすると、ストア設定の最後のタブに `Forms` があります。`Forms` をクリックして最初のカスタムフォームを作成します。

![BTCPay Server formbuilder - settings](./img/formbuilder/btcpayformbuilder1.png)

カスタムフォームページで `Create New Form` をクリックします。
`Email` と `Address` の2つのサンプルが用意されています。
この例では `Address` フォームをクリックします。

![BTCPay Server formbuilder - Create new form](./img/formbuilder/btcpayformbuilder2.png)

これで、BTCPay Server が用意したフィールドをドラッグして配置できます。
順序を入れ替えたり、フォーム下部の `Add form element` をクリックして新しいフィールドを作成できます。

## カスタムフォームを作成する

別のユースケースがあるかもしれません。たとえばレストランを運営している場合、テーブル番号、配膳時の呼び方、アレルギー、特別な要望などを把握したいことがあります。

次の手順でカスタムフォームを作成してみましょう。
前の例と同じく `Store settings -> Forms` から開始します。

右上の `create form` をクリックします。
まず名前を付けます。この例では `Restaurant` を使います。
前回と違い、今回は生成された空のフィールドから開始します。

![BTCPay Server formbuilder - Create new form](./img/formbuilder/btcpayformbuilder2-1.png)

1. 最初のフィールド名を `Table number` にします。
2. フィールドの `Type` を定義します。ここではテキストまたは数値が必要なので、ドロップダウンから `Number` を選択します。

![BTCPay Server formbuilder - Create new form](./img/formbuilder/btcpayformbuilder2-2.png)

3. このフィールドが顧客に表示されるラベルを設定します。この例では `Table Number` にします。
4. フィールド名は一貫性のため、前の項目と同じ `Table Number` を使います。
5. `Default value` は設定できますが、この例では空のままにします。
6. `Helper Text` は、作成中のフィールドの下に表示される補助テキストで、顧客に何を入力してほしいかを示します。
7. 最後に2つのパラメータを設定できます。1つは必須入力にする設定で、この例では有効にします。もう1つは `Constant` で、これを有効にするとユーザーが変更できないため、この例では使用しません。

![BTCPay Server formbuilder - Create new form](./img/formbuilder/btcpayformbuilder2-3.png)

フィールドのパラメータを入力すると、エディタ左側に顧客操作時の表示と動作が反映されます。

![BTCPay Server formbuilder - Create new form](./img/formbuilder/btcpayformbuilder2-4.png)

最初のフィールドが完成したら、下にある `+ Add form element` をクリックして必要な残りのフィールドを作成できます。すべて作成したら画面右上の `Save` をクリックして完了です。

![BTCPay Server formbuilder - Create new form](./img/formbuilder/btcpayformbuilder3.png)

`Form Builder` を使うと、柔軟かつ簡単にカスタムフォームを作成できます。さらに細かくカスタマイズしたい場合は、冒頭で触れた [Advanced Forms](./AdvancedForms.md) を参照し、フォームビルダーの `Code` タブで生成される JSON を確認してください。

## 公開フォーム

`Allow form for public use` を有効にすると、フォームを共有 URL として利用でき、ユーザーがフォーム入力後に請求書が生成されます。

デフォルトでは請求書通貨はストアの既定通貨、金額は "any" に設定されています。

特定の名前のフィールドを作成することで、事前設定済みの通貨と金額をフォームに持たせることができます。

* Invoice currency: フィールド名を `invoice_currency` にします。値が有効な通貨コードを返すようにしてください。
* Invoice amount: フィールド名を `invoice_amount` にします。値が数値を返すようにしてください。

これらのフィールドは `hidden` タイプにするとユーザーに表示しないようにできます。さらにユーザーに値を変更させたくない場合は、`Constant` を有効化してください。

これは Pay Button の代替として利用でき、金額や通貨などの請求パラメータを固定できる利点があります。

## ユーザー入力に応じて請求額を調整する

現代の多くの EC シナリオでは、配送方法、国、プロモーションコードなどのユーザー入力に応じて請求額を変更する必要があります。

BTCPay Server v1.11.0 以降、この機能がフォームに搭載されています。フィールド名が `invoice_amount_adjustment`（v1.11 以降対応）または `invoice_amount_multiply_adjustment`（v1.12 以降対応）で始まり、値が有効な数値であれば、請求額が自動的に調整されます。

この機能は現在、公開フォーム利用時と Point of Sale プラグインで動作します。
詳細は Advanced Forms ガイドの [Mirror Fields](./AdvancedForms.md#mirror-fields) セクションを参照してください。

### 配送方法に応じて追加料金を請求する

"select" タイプのフィールドを作成し、名前を `invoice_amount_adjustment_shipping_method` にします。利用可能な配送方法に対応する選択肢を作成します。ここでは `DHL` の値を `10`、`Fedex` の値を `20` とします。ユーザーがいずれかを選ぶと、請求額はそれぞれ 10 または 20 調整されます。

注: これは単純化した例です。請求額自体は正しく調整されますが、作成された請求書内に選択された配送方法は表示されません。これを実現するには `Mirror` フィールドを使う必要があります。

ユーザーが選択した配送方法を保存するには、代わりに次のように設定します。
* "select" タイプのフィールドを作成し、名前を `shipping_method` にします。配送方法に対応する選択肢を定義します。ここでは `DHL` の値を `dhl`、`Fedex` の値を `fedex` とします。
* "mirror" タイプのフィールドを作成し、名前を `invoice_amount_adjustment_shipping_method` にします。`Field to mirror` で `shipping_method` フィールドを選択し、`Value Mapper` に `shipping_method` の各選択肢と課金額を設定します。

### プロモーションコード

* "text" タイプのフィールドを作成し、名前を `promoCode` にします。
* "mirror" タイプのフィールドを作成し、名前を `invoice_amount_adjustment_promo` にします。`Field to mirror` で `promoCode` フィールドを選択し、`Value Mapper` に利用可能なプロモーションコードを作成します。例えば `Original Value` を `chocolate`、`Mapped Value` を `-5` に設定します。

ユーザーがプロモーションコード欄に `chocolate` と入力すると、請求額が -5 調整されます。

### レシートにユーザー入力を表示する

デフォルトでは、ユーザー入力は請求書レシートに表示されません。表示するには、各フィールドに対してマッピングを作成する必要があります。
* `fieldset` タイプのフィールドを作成し、名前を `receiptData` にします。
* レシートに表示したい各フィールドについて、`mirror` タイプのフィールドを作成し、`Field to mirror` にレシートへコピーしたいフィールドを設定します。
