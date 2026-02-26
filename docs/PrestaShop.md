# PrestaShop 連携

このドキュメントでは、**BTCPay Server を PrestaShop ストアに連携する方法**を説明します。  
まだストアがない場合は、[このステップバイステップの記事](https://blog.templatetoaster.com/how-to-install-prestashop/)に従ってゼロから作成してください。

既存の PrestaShop ストアに BTCPay Server を連携するには、以下の手順に従ってください。

:::tip
このドキュメントはモジュールの最新 _v6_ バージョンにのみ適用されます。その他のバージョン:
- [_v4_ module documentation](https://github.com/btcpayserver/btcpayserver-doc/blob/cba96292ceea9483711ab53c479a98357383f857/docs/PrestaShop.md)
- [_v5_ module documentation](https://github.com/btcpayserver/btcpayserver-doc/blob/b1432054e147836d7286e1bae2f98e62f2752363/docs/PrestaShop.md)
:::

## サーバー要件

このプラグインをインストールする前に、以下の要件を満たしていることを確認してください。

- PHP 8.0 以上を使用している
- PrestaShop が 8.0 以上である
  - ストアで HTTPS が有効になっており、公開アクセス可能であること
- BTCPay Server が 1.7.0 以上である
- PHP 拡張の PDO、curl、gd、intl、json、mbstring が利用可能である
- [セルフホスト](/Deployment/README.md) または [サードパーティホスト](/Deployment/ThirdPartyHosting.md) の BTCPay Server を用意している
  - BTCPay Server インスタンスで HTTPS が有効になっており、公開アクセス可能であること
- [インスタンスに登録済みアカウントがある](./RegisterAccount.md)
- [インスタンスに BTCPay ストアがある](./CreateStore.md)
- [ストアにウォレットが接続されている](./WalletSetup.md)

## BTCPay プラグインをインストールする

1. [最新の BTCPay Server プラグインをダウンロード](https://github.com/btcpayserver/prestashop-plugin/releases)
2. PrestaShop > Modules > Module Manager > Upload a module
3. 先ほどダウンロードした `.zip` ファイルをアップロード
4. `configure` をクリックしてモジュールを設定

![BTCPay Server PrestaShop plugin installation](./img/prestashop/module-install.jpg)

## ストアを接続する

Prestashop の BTCPay Server モジュールは、**あなたのサーバー（決済プロセッサ）と EC ストアをつなぐブリッジ**です。  
手順 2 でセルフホストかサードパーティホストのどちらを選んでも、設定手順は同じです。

1. `BTCPay Server URL` フィールドにホストの完全 URL（https を含む）を入力します。例: https://testnet.demo.btcpayserver.org
2. デフォルトのトランザクション速度を選択します（BTCPay が推奨する手数料額に影響します）。
3. _任意: ストアに適した注文モード（支払い前に注文作成 / 支払い後に注文作成）を選択します。_
   - この項目は、該当ロジックが削除された v5.1.0 **以前**を使用している場合のみ関係します。
4. 記帳用途のために、顧客メタデータを BTCPay Server インスタンスへ送信するかどうかを選択します。
5. `Connect` を押して設定を保存し、BTCPay Server インスタンスへリダイレクトして API キーを作成します。
6. API キー作成時は、必ず特定の 1 ストアに対して権限を付与してください（複数ストアは未対応です）。

![BTCPay Server PrestaShop API key setup](./img/prestashop/api-key-setup.jpg)

7. `Authorize app` ボタンを押すと Prestashop ストアへ戻ります。"Invalid Token" ポップアップが出る場合は、PrestaShop と BTCPay Server の両方で HTTPS が有効で、正しいホスト名を使っているか確認してください（[Server Requirements](#サーバー要件) を参照）。

![Invalid Token](./img/prestashop/invalid-token-popup.jpg)

8. Prestashop が BTCPay Server インスタンスへの接続を試みます。
9. 接続に成功するとメッセージが表示されます（ただし、テスト購入を行うことをおすすめします）。

![BTCPay Server PrestaShop setup finished](./img/prestashop/success.jpg)

:::tip
BTCPay Server からのリダイレクトは、PrestaShop 側の挙動により失敗することがあります。その場合でも、`/account/apikeys` から API キーをコピーしてフォームに貼り付ければ、このプラグインを利用できます。
:::

### API キーを手動で作成する

必要に応じて、`/account/addapikey` で API キーを手動作成することもできます。手動で作成する場合は、_単一_ のストアに対して次の権限を持つようにしてください。

- `btcpay.store.canmodifystoresettings`
- `btcpay.store.webhooks.canmodifywebhooks`
- `btcpay.store.canviewstoresettings`
- `btcpay.store.cancreateinvoice`
- `btcpay.store.canviewinvoices`
- `btcpay.store.canmodifyinvoices`

## 3. 貢献

BTCPay は世界中のボランティアコントリビューターによって構築・保守されています。新しい貢献を歓迎し、感謝しています。

貢献したい方は、プルリクエストを開く前に、まず [issue を作成](https://github.com/btcpayserver/prestashop-plugin/issues/new/choose) するか、[コミュニティチャット](https://chat.btcpayserver.org) に参加してください。早い段階でフィードバックを得たり、最適な進め方を議論したり、作業重複を防げます。

## PrestaShop サポート

PrestaShop のサポートは公式チャネルで提供されています。

- [Homepage](https://www.prestashop.com)
- [Documentation](https://doc.prestashop.com)
- [Support Forums](https://www.prestashop.com/forums)
