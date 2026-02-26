# Pretix - イベント向けチケット販売ソフトウェア

[Pretix](https://pretix.eu/) は、カンファレンス、フェスティバル、展示会、ワークショップなどのイベント向けの、無料で [オープンソースのチケット販売ソフトウェア](https://github.com/pretix/pretix) です。自分でデプロイすることも、最大 2500 枚のチケット販売まで無料の [クラウド版](https://pretix.eu/about/en/pricing) から始めることもできます。

:::tip
この連携は Pretix によって保守されており、BTCPay Server プロジェクトの一部ではありません。
:::

# 要件
- [Pretix cloud account](https://pretix.eu/signup/) または [self-hosted instance](https://docs.pretix.eu/en/latest/admin/installation/index.html) を持っている
- Pretix をセルフホストしている場合、[BitPay plugin](https://github.com/pretix/pretix-bitpay) を手動でインストールする必要がある
- BTCPay Server 1.15.0 以上を利用している（[self-hosted](/Deployment/README.md) または [hosted by a third-party](/Deployment/ThirdPartyHosting.md)）
- [インスタンスに登録済みアカウントがある](./RegisterAccount.md)
- [インスタンスに BTCPay ストアがある](./CreateStore.md)
- [ストアにウォレットが接続されている](./WalletSetup.md)

## Pretix 用 BitPay プラグインのインストールと設定

:::tip
プラグイン名は BitPay ですが、BTCPay Server インスタンスを指すカスタムドメインを設定できるため、実際には BTCPay Server もサポートしています。
:::

1. Pretix ダッシュボードで設定したいイベントを選択します。
2. 左サイドバーで "Settings" を展開し、"Plugins" をクリックします。
3. 上部の "Payment providers" タブを選択します。
4. "BitPay" プラグインを見つけて "Enable" をクリックします。
![Enable BitPay plugin](./img/pretix/pretix-step-1-4.png)
----
5. 左サイドバーで "Payment" をクリックします。
![Payment](./img/pretix/pretix-step-5.png)
----
6. "BitPay" の行で "Settings" をクリックします。
![BitPay settings](./img/pretix/pretix-step-6.png)
----
7. BTCPay Server インスタンスの URL を入力します。例: `https://mainnet.demo.btcpayserver.org`（このインスタンス上に BTCPay ストアを作成している必要があります。上記 [Requirements](#要件) を参照）。
8. "Start pairing" をクリックします。
![Start pairing process](./img/pretix/pretix-step-7-8.png)
----
9. BTCPay Server の "pairing permission" ページへリダイレクトされます。ペアリングするストアを選択します。
10. "Approve" をクリックします。
![Approve pairing](./img/pretix/pretix-step-9-10.png)
----
11. "Pairing successful" と表示されます。Pretix へ自動で戻らないため、このタブは閉じて構いません。
![Pairing successful](./img/pretix/pretix-step-11.png)
----
12. Pretix に戻り、右下の "Save" をクリックします。
![Save settings](./img/pretix/pretix-step-12.png)
----
13. "Payment settings" ページが表示されます。上部の **Enable payment method** をチェックします。
14. **Payment method name**: 支払い方法名を入力します。例: "Bitcoin / Lightning Network"。その他の項目はそのままでも、必要に応じて調整しても構いません。
15. 右下の "Save" をクリックします。
![Payment settings](./img/pretix/pretix-step-13-15.png)

これで設定は完了です。

テスト購入を実行して、支払い方法が期待どおり動作することを確認してください。
