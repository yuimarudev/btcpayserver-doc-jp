# ダッシュボード

BTCPay Server version 1.5.0 では、新しいダッシュボードのコンセプトが導入されました。複数のタイルによって、初期設定の補助、ストアデータの理解、返金と支払いの管理をより簡単に行えます。

![BTCPay Server Navigation](./img/dashboard/dashboardgif.gif)

## ダッシュボードタイル

メインのダッシュボード画面には、ストアのパフォーマンスを素早く把握できるタイルがいくつかあります。

### ウォレット残高

現在のストアの [wallet](Wallet.md) 残高を表示し、週・月・年単位のグラフを確認できます。

![BTCPay Server Navigation](./img/dashboard/wallet-view.jpg)

### トランザクションアクティビティ

未処理の支払いをすばやく管理し、最近のトランザクションを確認し、未処理の [refunds](Refund.md) を一覧できます。

![BTCPay Server Navigation](./img/dashboard/tx-activity-view.jpg)

### Lightning 残高

Lightning ノードの利用可能残高を表示します。
オンチェーン残高は、ストア全体のオンチェーンウォレットではなく、ストアの Lightning ノードのウォレットを指します。

`Node Info` には、ノードの概要、オンライン状態、ピア接続に使うアドレスが表示されます。
`Lightning Network` の詳細は [Lightning Network page](./LightningNetwork.md) を確認してください。

![BTCPay Server Dashboard LN](./img/dashboard/btcpayLNDashboard3.jpg)

### Lightning サービス

このタイルには、次の `Lightning Network` サービスへのクイックボタンがあります。

- Core Lightning (REST)
- Ride The Lightning
- ThunderHub
- Lightning Terminal

![BTCPay Server Dashboard LN](./img/dashboard/btcpayLNDashboard4.jpg)

### 最近のトランザクション

オンチェーンウォレットに到着した直近 5 件のトランザクションを表示します。

![BTCPay Server Navigation](./img/dashboard/recent-tx-view.jpg)

### 最近の請求書

直近 5 件の請求書がステータスと金額とともに表示され、特定の [invoice](/Invoices.md) に素早くアクセスして管理できます。

![BTCPay Server Navigation](./img/dashboard/recent-invoice-view.jpg)

### 現在アクティブなクラウドファンド

このタイルには、現在アクティブなクラウドファンドと、その上位アイテム/特典が表示されます。複数のクラウドファンディングアプリがアクティブな場合、最初のタイルの下に追加表示されます。アクティブなクラウドファンドキャンペーンを管理し、すべての特典とその成果を確認する簡単な方法です。

![BTCPay Server Navigation](./img/dashboard/fund-full-view.jpg)

:::warning
このページは、ソフトウェアの進展に伴って変更される可能性があります。機能はリリースごとに更新されます。
:::
