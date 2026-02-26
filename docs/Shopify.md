# BTCPay Server で Shopify での Bitcoin 決済を受け付ける

Shopify 向け BTCPay Server をご紹介します。これはオープンソースの決済ゲートウェイで、手数料なしで、あなたの Web サイトやストアで顧客からの bitcoin 決済を直接受け付けられます。

Shopify との連携により、セルフホストした BTCPay Server を [Shopify ストア](https://www.shopify.com/)に接続し、Bitcoin 決済を迅速かつ安全に受け付けられます。

:::warning

⚠️ Shopify V1 連携は非推奨です

新規ユーザーは、BTCPay Server の Shopify V1 連携を利用できなくなりました。BTCPay Server を Shopify と連携する設定手順は、[Shopify V2 integration guide](/ShopifyV2.md) を参照してください。  
V1 連携で問題が発生している既存ユーザーは、[Github repository](https://github.com/btcpayserver/btcpayserver) に issue を作成するか、[Mattermost](http://chat.btcpayserver.org/) または [Telegram](https://t.me/btcpayserver) で報告し、V2 への移行計画を立てることをおすすめします。
:::


## BTCPay でできること:

- **手数料ゼロ**: 手数料のない決済ゲートウェイを利用できます。本当にゼロです。
- **中間業者や KYC なしの直接決済**: 仲介業者や煩雑な書類対応は不要で、資金をウォレットへ直接受け取れます
- **完全自動化されたシステム**: BTCPay が決済、請求書管理、返金を自動で処理します。
- **チェックアウト時に Bitcoin QR コードを表示**: 簡単かつ安全な決済方法で顧客体験を向上できます。
- **セルフホスト基盤**: 決済ゲートウェイを完全にコントロールできます。
- **Lightning Network 統合**: 即時で高速、低コストな決済と支払い
- **簡単な CSV エクスポート**
- **柔軟なプラグインシステム**: ニーズに応じて機能を拡張できます
- **POS 連携** – 実店舗でも決済を受け付け可能
- **多言語対応**: 初期状態でグローバルなユーザーに対応できます。
- **コミュニティ主導のサポート**: 専用コミュニティから迅速な支援を受けられます（[Mattermost](http://chat.btcpayserver.org/) または [Telegram](https://t.me/btcpayserver)）。


## 前提条件:

設定を始める前に、次のものを用意してください:

- Shopify アカウント
- BTCPay Server - [self-hosted](/Deployment/) または [third-party host](/Deployment/ThirdPartyHosting.md) で稼働する v1.4.8 以降
- [Created BTCPay Server store](CreateStore.md) と [wallet set up](WalletSetup.md)

[![BTCPay Server - Shopify Video](https://img.youtube.com/vi/jJjAyvgWVfk/mqdefault.jpg)](https://www.youtube.com/watch?v=jJjAyvgWVfk)


## BTCPay Server を Shopify と連携する

1. Shopify で、左サイドバーの `Apps >` をクリックします
2. 表示されたモーダルで `App and sales channel settings` をクリックします
3. 表示されたページで `Develop apps` ボタンをクリックします
4. 求められた場合は `Allow custom app development` をクリックします
5. `Create an app` を選び、例として BTCPay Server などの名前を付けます
6. アプリページの `Overview` タブで `Configure Admin API scopes` をクリックします
7. admin access scopes のフィルターに `Orders` と入力します
8. `Orders` で `read_orders` と `write_orders` を有効にし、`Save` をクリックします
9. 右上の `Install App` をクリックし、ポップアップが表示されたら `Install` をクリックします
10. `Admin API access token` を表示して `copy` します
11. BTCPay Server 側でストアを開き、左サイドバーの `Shopify` をクリックします
12. 1つ目のフィールド `Shop name` に Shopify ストアのサブドメイン（例: SOME_ID.myshopify.com の SOME_ID）を入力します
13. 3つ目のフィールド `Admin API access token` に、Shopify でコピーした `Admin API access token` を貼り付けます
14. 2つ目のフィールド `API key` に Shopify の `API key` を貼り付けます。これは Admin API access token をコピーした同じページ下部で確認できます
15. BTCPay の Shopify 設定ページで `Save` をクリックします
16. Shopify に戻り、左メニューで `Checkout` を選択し、「Order status page」までスクロールして、BTCPay の Shopify 設定に表示される HTML `<script>` コードを「Additional scripts」テキストフィールドに貼り付けます
17. `Save` をクリックし、ページ上部まで戻ります
18. 左サイドバーの `Payments` をクリックし、「Manual payment methods」までスクロールして `(+) Manual payment method` をクリックし、ドロップダウンから `Create custom payment method` を選びます
19. `Custom payment method name` に `Bitcoin with BTCPay Server` を入力します。ほかのフィールドは任意入力で、必須ではありません
20. `Activate` をクリックすれば、Shopify と BTCPay Server の設定は完了です

:::tip
"Custom Payment method name" には、次の単語のいずれかを少なくとも1つ含める必要があります（大文字小文字は区別されません）: `bitcoin`, `btcpayserver`, `btcpay server` または `btc`。
:::

以下は、上記プロセスのステップごとの画面イメージです。

![BTCPay Server shopify step 1](./img/shopify/btcpayshopify1.png)

![BTCPay Server shopify step 2](./img/shopify/btcpayshopify2.png)

![BTCPay Server shopify step 3](./img/shopify/btcpayshopify3.png)

![BTCPay Server shopify step 4](./img/shopify/btcpayshopify4.png)

![BTCPay Server shopify step 5](./img/shopify/btcpayshopify5.png)

![BTCPay Server shopify step 6](./img/shopify/btcpayshopify6.png)

![BTCPay Server shopify step 7](./img/shopify/btcpayshopify7.png)

![BTCPay Server shopify step 8](./img/shopify/btcpayshopify8.png)

![BTCPay Server shopify step 9](./img/shopify/btcpayshopify9.png)

![BTCPay Server shopify step 10](./img/shopify/btcpayshopify10.png)

![BTCPay Server shopify step 11](./img/shopify/btcpayshopify11.png)

![BTCPay Server shopify step 12](./img/shopify/btcpayshopify12.png)

![BTCPay Server shopify step 13](./img/shopify/btcpayshopify13.png)

![BTCPay Server shopify step 14](./img/shopify/btcpayshopify14.png)

![BTCPay Server shopify step 14-2](./img/shopify/btcpayshopify14-2.png)

すべての設定完了後のデモ Checkout フロー:

![BTCPay Server shopify step 15](./img/shopify/btcpayshopify15.png)

![BTCPay Server shopify step 16](./img/shopify/btcpayshopify16.png)

![BTCPay Server shopify step 17](./img/shopify/btcpayshopify17.png)

![BTCPay Server shopify step 18](./img/shopify/btcpayshopify18.png)
