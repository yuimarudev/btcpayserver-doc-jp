# 請求書メタデータ

各請求書にはメタデータが含まれます。これは請求書作成時に API 経由で調整できる、カスタマイズ可能な JSON オブジェクトです。固定スキーマはありませんが、メタデータ内の一部プロパティは UI 側で解釈されます。

このページでは、これらのプロパティの概要と BTCPay Server 内での利用方法を説明します。

## よく知られたプロパティ

| プロパティパス      | 説明                                                                                                                                                                                                                                                            |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.orderId`          | WooCommerce のような外部システムの注文 ID を指します。このプロパティはインデックス化されており、`orderId` を使った請求書検索を効率的に行えます。                                                                                                            |
| `.orderUrl`         | 外部システムの注文ページへ戻る URL を指します。このリンクは請求書詳細ビューに表示されます。                                                                                                                                                                  |
| `.paymentRequestId` | 請求書詳細ビューで、該当請求書に関連する payment request ページへ移動するリンクが提供されます。                                                                                                                                                               |
| `.posData`          | 請求書詳細ビューに表示される情報を表すカスタム JSON オブジェクトです。                                                                                                                                                                                        |
| `.receiptData`      | 請求書のレシートページに表示される情報を表すカスタム JSON オブジェクトです。                                                                                                                                                                                  |
| `.buyerName`        | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerEmail`       | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerAddress1`    | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerAddress2`    | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerCity`        | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerState`       | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerZip`         | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerCountry`     | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.buyerPhone`       | 請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                                                           |
| `.itemDesc`         | Point of Sale（keypad / cart view を除く）使用時に、購入商品の説明がこのフィールドへ設定されます。この情報は CSV 請求書エクスポート機能に含まれ、請求書詳細ビューにも表示されます。                                                                          |
| `.itemCode`         | Point of Sale（keypad / cart view を除く）使用時に、購入商品の商品コードがこのフィールドへ設定されます。この情報は CSV 請求書エクスポート機能に含まれ、請求書詳細ビューにも表示されます。                                                                    |
| `.physical`         | 物理商品かどうかを示す Boolean 値です。請求書詳細ビューおよび BitPay API 互換エンドポイントで表示されます。                                                                                                                                                  |
| `.taxIncluded`      | 請求書通貨での税額を表す数値です。この情報は請求書詳細ビューに表示されます。請求書作成時には有効桁へ自動丸めされ、請求書価格を超えないことが保証されます。                                                                                                  |

## 例

Point of sale invoice（Product list view）:

```json
{
  "orderId": "pos-app_346KRC5BjXXXo8cRFKwTBmdR6ZJ4",
  "itemCode": "green tea",
  "itemDesc": "Green Tea",
  "orderUrl": "https://localhost:14142/apps/346KRC5BjXXXo8cRFKwTBmdR6ZJ4/pos",
  "receiptData": {
    "Title": "Green Tea",
    "Description": "Lovely, fresh and tender, Meng Ding Gan Lu ('sweet dew') is grown in the lush Meng Ding Mountains of the southwestern province of Sichuan where it has been cultivated for over a thousand years."
  }
}
```

Point of sale invoice（Cart view）:

```json
{
  "orderId": "pos-app_346KRC5BjXXXo8cRFKwTBmdR6ZJ4",
  "posData": {
    "tip": 0.48,
    "cart": [
      {
        "id": "pu erh",
        "count": 1,
        "image": "~/img/pos-sample/pu-erh.jpg",
        "price": {
          "type": 2,
          "value": 2,
          "formatted": "$2.00"
        },
        "title": "Pu Erh",
        "inventory": null
      },
      {
        "id": "rooibos",
        "count": 1,
        "image": "~/img/pos-sample/rooibos.jpg",
        "price": {
          "type": 2,
          "value": 1.2,
          "formatted": "$1.20"
        },
        "title": "Rooibos",
        "inventory": null
      }
    ],
    "total": 3.68,
    "subTotal": 3.2,
    "customAmount": 0,
    "discountAmount": 0,
    "discountPercentage": 0
  },
  "itemDesc": "Tea shop",
  "orderUrl": "https://localhost:14142/apps/346KRC5BjXXXo8cRFKwTBmdR6ZJ4/pos",
  "receiptData": {
    "Tip": "$0.48",
    "Cart": {
      "Pu Erh": "$2.00 x 1 = $2.00",
      "Rooibos": "$1.20 x 1 = $1.20"
    }
  }
}
```

Point of sale invoice（Keypad view）:

```json
{
  "orderId": "pos-app_346KRC5BjXXXo8cRFKwTBmdR6ZJ4",
  "posData": {
    "total": "12.00",
    "subTotal": "12.00"
  },
  "itemDesc": "Tea shop",
  "orderUrl": "https://localhost:14142/apps/346KRC5BjXXXo8cRFKwTBmdR6ZJ4/pos",
  "receiptData": {}
}
```
