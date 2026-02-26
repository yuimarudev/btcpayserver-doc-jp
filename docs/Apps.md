---
description: Apps は BTCPay Server を簡単に拡張できる機能です。
tags:
  - BTCPay Server
  - Apps
  - Point of Sale
  - Crowdfunding
  - Payment Button
---

# BTCPay Server Apps

BTCPay Server の主な目的は、信頼できる第三者への依存をなくすことです。Apps は中央集権的な権限を不要にし、ソフトウェアの[ユースケース](./UseCase.md)を簡単に拡張できる組み込みアプリケーションです。ユーザーは、すぐに使えるさまざまなカスタマイズ可能アプリをセルフホストできます。

アプリを作成するには、Apps > Create a new app に進みます。Apps はストアに紐づくため、各アプリをストアに接続する必要があります。

## Point of Sale App

**Web ベースの PoS アプリ**を使うと、実店舗を持つユーザーは**手数料や第三者を介さずに暗号通貨を受け取り**、直接自分のウォレットに入金できます。**PoS** はタブレットや Web 閲覧可能なデバイスに簡単に表示できます。ホーム画面ショートカットを作れば、Web アプリへすぐアクセスできます。

![BTCPay Pos](./img/BTCPayPointOfSale1.jpg)

新しい商品追加は簡単です。このアプリには**ショッピングカート機能**、**チップ**、**在庫管理**、**カスタム支払いオプション**などがあります。

**Point of Sale app** は、設定やカスタマイズ次第で、寄付・チップの受け取りや小規模 EC ショップとしても利用できます。

現在、**Point of Sale app** は次の 3 つの表示モードに対応しています。

- `Static` ビュー: 販売商品だけを表示
- `Cart` ビュー: 商品一覧とチェックアウト用カートを表示
- `Light` ビュー: すばやい支払い向けのキーパッドのみを表示（[v1.0.5.6](https://blog.btcpayserver.org/btcpay-server-1-0-5-6/#simplePOS) 以降）

最初の **Point of Sale app** を動かすには、以下の手順に従ってください。

1. `Apps` で `Create a new app` をクリック
2. アプリの `name` を入力
3. `app type` > Point Of Sale を選択
4. アプリに紐づける `store` を選択
5. `view`（Static / Cart / Light）を選び、価格・写真・説明付きで `items` を追加して PoS をカスタマイズ
6. `Save Settings` をクリック
7. `View App` をクリックして PoS を表示（顧客はこのリンクから PoS にアクセス可能）

**Point of Sale app** の見た目は、[theme customization guide](./Development/Theme.md) に従って変更できます。

## Crowdfunding App

**Crowdfunding** は、BTCPay Server の画面から起動できるアプリで、Kickstarter や Indiegogo のような**セルフホスト型資金調達キャンペーン**を作成できます。従来の**クラウドファンディングプラットフォーム**と異なり、キャンペーン作成者自身がプラットフォームの所有者です。資金は**手数料なしで**作成者のウォレットへ直接入金されます。

1. Apps に移動
2. アプリ名を入力
3. app type > Crowdfund を選択
4. 紐づけるストアを選択
5. 価格・写真・説明付きで特典（perks）を追加して Crowdfund をカスタマイズ
6. Allow crowdfund to be publicly visible をチェック
7. `Save Settings` をクリック
8. `View App` をクリックして Crowdfund を表示（支援者はこのリンクからアクセス可能）

[![BTCPay Server Crowdfunding](https://img.youtube.com/vi/tFbfyneDj88/mqdefault.jpg)](https://www.youtube.com/watch?v=tFbfyneDj88)

**クラウドファンディングキャンペーン**の支援者にデジタル商品や物理商品を提供したい場合は、[WooCommerce ストアを連携](./FAQ/Apps.md#how-to-integrate-woocommerce-store-into-a-btcpay-crowdfund-app)できます。在庫機能を使って特典の上限数も設定できます。

## Payment Button

埋め込みが簡単な HTML で高いカスタマイズ性を持つ**payment button** を使うと、チップや寄付を受け取れます。オンラインストアにも統合できます。サイト訪問者がボタンをクリックすると、BTCPay が**invoice** を表示します。

1. 左メニューの "PLUGINS" セクションで "Pay Button" を選択
2. 誰でも請求書を作成できるように設定
3. ボタンをカスタマイズ
4. 生成されたフォームをコピーして Web サイトに埋め込む

[![BTCPay Server Payment Buttons](https://img.youtube.com/vi/MIWGvl6_WzI/mqdefault.jpg)](https://www.youtube.com/watch?v=MIWGvl6_WzI)
