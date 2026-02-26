<!-- legacy-anchor-aliases -->
<span id="mirror-fields"></span>
<!-- /legacy-anchor-aliases -->

# フォーム

BTCPay Server の Forms 機能を使うと、支払いに進む前に顧客へフォーム入力を求められます。

これらのフォームは要件に合わせて完全にカスタマイズできます。

フォーム定義の例:

```json
{
  "fields": [
    {
      "name": "buyerEmail",
      "constant": false,
      "type": "email",
      "value": null,
      "required": true,
      "label": "Enter your email",
      "helpText": "This is help text",
      "fields": []
    },
    {
      "name": "buyerName",
      "constant": false,
      "type": "text",
      "value": null,
      "required": true,
      "label": "Name",
      "helpText": null,
      "fields": []
    },
    {
      "name": "buyerAddress1",
      "constant": false,
      "type": "text",
      "value": null,
      "required": true,
      "label": "Address Line 1",
      "helpText": null,
      "validationErrors": [],
      "fields": []
    },
    {
      "name": "buyerAddress2",
      "constant": false,
      "type": "text",
      "value": null,
      "required": false,
      "label": "Address Line 2",
      "helpText": null,
      "fields": []
    },
    {
      "name": "buyerCity",
      "constant": false,
      "type": "text",
      "value": null,
      "required": true,
      "label": "City",
      "helpText": null,
      "fields": []
    },
    {
      "name": "buyerZip",
      "constant": false,
      "type": "text",
      "value": null,
      "required": false,
      "label": "Postcode",
      "helpText": null,
      "fields": []
    },
    {
      "name": "buyerState",
      "constant": false,
      "type": "text",
      "value": null,
      "required": false,
      "label": "State",
      "helpText": null,
      "fields": []
    },
    {
      "name": "buyerCountry",
      "constant": false,
      "type": "text",
      "value": null,
      "required": true,
      "label": "Country",
      "helpText": null,
      "fields": []
    }
  ]
}
```

出力:

![Form](./img/Forms-1.png)

フィールド定義では、次の項目のみ設定できます。

| 項目                   | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `.fields.constant`      | `true` の場合、フォーム定義で `.value` を設定する必要があり、ユーザーはそのフィールド値を変更できません。（例: フォーム定義のバージョン）                                                                                                                                                                                                                                                                                                   |
| `.fields.type`          | HTML の input type: `text`, `checkbox`, `password`, `hidden`, `color`, `date`, `datetime-local`, `month`, `week`, `time`, `email`, `number`, `url`, `tel`                                                                                                                                                                                                                                                          |
| `.fields.options`       | `.fields.type` が `select` の場合に選択可能な値の一覧                                                                                                                                                                                                                                                                                                                                                                                                       |
| `.fields.options.text`  | このオプションで表示されるテキスト                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `.fields.options.value` | このオプションが選択されたときのフィールド値                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `.fields.type=fieldset` | 子要素 `.fields.fields`（下記参照）を囲む HTML の `fieldset` を作成                                                                                                                                                                                                                                                                                                                                                                                          |
| `.fields.name`          | 請求書メタデータ内で使われる、このフィールドの JSON プロパティ名                                                                                                                                                                                                                                                                                                                                                                                    |
| `.fields.value`         | フィールドのデフォルト値                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `.fields.required`      | `true` の場合、入力必須になります                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `.fields.label`         | フィールドのラベル（整形やリンク用の HTML を含められます）                                                                                                                                                                                                                                                                                                                                                                                                 |
| `.fields.helpText`      | フィールドの説明として表示する追加テキスト（整形やリンク用の HTML を含められます）                                                                                                                                                                                                                                                                                                                                                                |
| `.fields.fields`        | `.fields.type` が `fieldset` の場合、階層構造でフィールドを整理でき、子フィールドを請求書メタデータ内にネストできます。この構造により、収集した情報の整理・管理がしやすくなり、アクセスや解釈も容易になります。たとえば顧客情報を収集するフォームでは、`customer` という親フィールド配下に `name`、`email`、`address` などの子フィールドを配置できます。 |
| `.fields.valuemap`      | `.fields.type` が `mirror` の場合、キーが一致条件、値が変換結果となるオブジェクトを指定できます。`{ "hello": "world"}` は、コピー元が `hello` のとき `world` として保存されることを意味します。 |

フィールド値は[請求書のメタデータ](/Development/InvoiceMetadata.md)に保存されます。

## よく使われるフィールド名

フィールド名は、請求書メタデータにユーザー入力値を保存する JSON プロパティ名を表します。

いくつかの既知の名前は解釈され、請求書設定を変更できます。

| フィールド名         | 説明            |
| ------------------ | ---------------------- |
| `invoice_amount`   | 請求書の金額   |
| `invoice_currency` | 請求書の通貨 |
| `invoice_amount_adjustment` で始まる名前 | 値が数値として評価できる限り、その値に応じて請求書金額を調整します。 |
| `invoice_amount_multiply_adjustment` で始まる名前 | この値を掛けることで、生成された請求書金額を調整します。 |

## ミラーフィールド

`Mirror` フィールドは型 `mirror` で定義します。値には別フィールド名を設定し、フォーム送信時にそのフィールド値をミラーフィールドへコピーします。
`mirror` 型には値マッピング機能もあり、参照先フィールドの値を変換してミラーフィールドへコピーできます。

たとえば、国一覧の select フィールドを用意し、`invoice_amount_adjustment` フィールドを作ることで、選択された国に応じて請求書価格を調整できます。
また、`invoice_amount_multiply_adjustment` フィールドを使って、割合ベースのプロモコードを生成することもできます。

以下は、割引率の異なる3つのプロモコード実装例です。

- `huge` = 50% 割引
- `medium` = 10% 割引
- `tiny` = 1% 割引

```json
{
  "fields": [
    {
      "name": "promo",
      "type": "text",
      "label": "Promo Code"
    },
    {
      "name": "invoice_amount_multiply_adjustment_promo",
      "type": "mirror",
      "value": "promo",
      "label": "Promo Codes",
      "valuemap": {
        "tiny": "0.99",
        "medium": "0.90",
        "huge": "0.5"
      }
    }
  ]
}
```

## フォーム値の事前入力

`?your_field=value` のようなクエリ文字列をフォーム URL に追加することで、請求書フィールドを自動で事前入力できます。

この機能の利用例:

- `ユーザー入力補助`（Assisting user input）: 既知の顧客情報を事前入力し、フォーム入力を簡単にします。たとえば顧客のメールアドレスが既知なら、メール欄を埋めて時間を短縮できます。
- `パーソナライズ`（Personalization）: 顧客の好みやセグメントに応じてフォームを調整できます。たとえば顧客ランクごとに会員レベルや特典情報を事前入力できます。
- `トラッキング`（Tracking）: 非表示フィールドと事前入力値を使って訪問元を追跡できます。たとえばマーケティングチャネル（Twitter、Facebook、email）ごとに `utm_media` を埋めたリンクを作成し、施策効果を分析できます。
- `A/B テスト`（A/B testing）: 異なる値を事前入力したフォームを使い分けることで、ユーザー体験やコンバージョン率を最適化できます。
