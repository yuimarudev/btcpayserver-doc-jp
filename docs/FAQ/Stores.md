<!-- legacy-anchor-aliases -->
<span id="add-network-fee-to-invoice-vary-with-mining-fees"></span>
<span id="allow-anyone-to-create-invoice"></span>
<span id="can-i-delete-invoices-from-btcpay"></span>
<span id="consider-the-invoice-confirmed-when-the-payment-transaction"></span>
<span id="consider-the-invoice-paid-even-if-the-paid-amount-is-less-than-expected"></span>
<span id="getting-getratesasync-was-called-on-coinaverage-error"></span>
<span id="how-many-stores-can-i-create"></span>
<span id="how-to-change-the-exchange-rate-provider-for-invoices"></span>
<span id="how-to-collect-additional-buyer-information"></span>
<span id="how-to-create-a-store-in-btcpay"></span>
<span id="how-to-denominate-invoices-in-sats"></span>
<span id="how-to-disable-email-on-invoices"></span>
<span id="how-to-redirect-store-invoices-after-payment"></span>
<span id="invoice-expires-if-the-full-amount-has-not-been-paid-after-minutes"></span>
<span id="payment-invalid-if-transactions-fails-to-confirm--minutes-after-invoice-expiration"></span>
<span id="payment-invalid-if-transactions-fails-to-confirm-minutes-after-invoice-expiration"></span>
<span id="store-general-settings"></span>
<span id="why-are-invoices-without-payment-showing-as-complete"></span>
<!-- /legacy-anchor-aliases -->

# ストア FAQ

このページでは、BTCPay Server のストアに関するよくある問題と質問をまとめています。

[[toc]]

## BTCPay Server でストアを作成するには？

最初のストアを作成するには、ヘッダーメニューの `Stores` に移動し、"create a new store" をクリックします。

## 作成できるストア数に制限はありますか？

BTCPay で作成できるストア数に制限はありません。

## 支払いのない請求書が完了済みとして表示されるのはなぜですか？

支払い金額が 0（支払額なし）で請求書が作成された場合、定義上その請求書はすでに支払い済みです。請求書は作成直後に完了として表示されます。

この種類の請求書は通常、加盟店がユーザーに資金提供を求めず、BTCPay Server の請求書を使ってイベントや配布企画への関心を確認したい場合に使われます。別の用途として、開発者が請求フローをテストする際、実際の資金を使わずにソフトウェアが正しく動作するか検証するケースがあります。

## 請求書にネットワーク手数料を追加する（マイニング手数料に応じて変動）には？

ネットワーク手数料（コスト）は、顧客の分割払いによる加盟店側の不利益を防ぐ BTCPay の機能です。請求書が多数の出力で支払われると、後で加盟店が資金を移動する際の手数料は高くなります。

例えば、顧客が 20 ドルの請求書を作成し、1 ドルずつ 20 回に分けて全額を支払ったとします。加盟店側では UTXO が増え、後で資金移動する場合のマイニングコストが上がります。デフォルトでは BTCPay はこの費用を補うため、請求総額に **追加のネットワークコスト** を加算します。

BTCPay には、この保護機能を調整する複数のオプションがあります。ネットワーク手数料は次のように設定できます。

- 顧客が同一請求書に対して 2 回以上支払った場合のみ適用（上記の例では、20 ドル請求書に 1 ドル支払った時点で残額は 19 ドル + ネットワーク手数料。ネットワーク手数料は **初回支払い後** に適用）
- すべての支払いに適用（初回支払いを含む。上記の例では、最初から 20 ドル + ネットワーク手数料）
- ネットワーク手数料を追加しない（機能を完全に無効化）

BTCPay のネットワーク手数料は **マイニング手数料そのものではありません**。顧客は引き続きマイナー手数料を支払う必要があります。

ネットワークコストは任意機能です。デフォルトで有効ですが、有効化・無効化は加盟店が自由に選べます。顧客には、請求情報を展開した際に "network cost" として表示されます。

この機能はダスト取引対策として有効ですが、説明が不十分だとビジネス上マイナスに働く可能性があります。顧客が追加料金について疑問を持ち、過剰請求と受け取る場合があります。

自社ビジネスへの影響を考慮し、ストアの利用規約などで適切に顧客へ案内することを推奨します。

## 誰でも請求書を作成できるようにするには？

外部ユーザーにストアで請求書を作成させたい場合は、このオプションを有効にします。この設定が有効なのは、支払いボタンの利用時、または API / サードパーティ HTML サイト経由で請求書を発行する場合です。PoS アプリは事前承認されているため、ランダムな訪問者が PoS ストアを開いて請求書を作成するのにこの設定は不要です。迷う場合は無効のままにしてください。必要になれば後から有効化できます。

## 全額支払われない場合、請求書は ... 分後に期限切れになります

請求書タイマーはデフォルトで 15 分です。これは価格変動対策として、暗号資産建て金額を法定通貨レートに基づいて固定するための保護機構です。顧客が定義時間内に支払わない場合、請求書は期限切れになります。請求書はトランザクションがブロックチェーン上で見えた時点（0-confirmations）で "paid" とみなされ、加盟店が定義した確認数（通常 1〜6）に達すると "complete" になります。タイマーは変更可能です。

## 請求書期限切れ後 ... 分以内にトランザクションが承認されない場合、支払いを無効にする

顧客が請求書を支払っても、設定期間内に定義した承認数へ到達しない場合、その支払いは "invalid" とマークされます。その後、加盟店は手動で受け入れるか、拒否して返金フローや追加支払い要求などの対応を行えます。これはレート変動を悪用した攻撃への保護機構です。このオプションは [ウォレット設定](../Wallet.md#settings) にあります。

## 支払いトランザクションが ... になった時点で請求書を承認済みとみなす

請求書は、ブロックチェーン（および mempool）上で確認できると "processing" になります。定義した承認数に達すると "settled" になります。ここでは請求書が "confirmed" になる最小承認数を設定します。これはオンチェーン支払いにのみ適用されます。Lightning Network で支払われた請求書は確認が即時のため、すぐに settled 状態になります。実運用では、加盟店は請求書が settled と表示された時点で商品発送するのが一般的です。詳細は [ウォレット設定](../Wallet.md#settings) を参照してください。

## 0-conf 設定時に RBF フラグ付きトランザクションでも請求書を承認済みにする

通常、請求書はブロックチェーン（mempool）上で見えた時点で "processing" になります。ただし、トランザクションが RBF としてフラグ付けされる場合があります。
BTCPay Server は RBF フラグを確認します。RBF が検出された場合、BTCPay Server は自動的に 1 confirmation を待機します。
詳細は [ウォレット設定](../Wallet.md#settings) を参照してください。

## 支払額が期待値より ... % 少なくても請求書を支払済みとみなす

顧客が取引所ウォレットから請求書へ直接支払うと、取引所側で少額手数料が差し引かれることがあります。その場合、請求書は全額完了とはみなされず "paid partially" になります。加盟店が不足支払いを受け入れたい場合、ここで許容割合を設定できます。

## 請求書のメール入力を無効化する方法

ストア請求書でメール必須を無効化するには、`Stores > Settings > Checkout Experience` へ進み、`Requires a refund email` のチェックを外します。

## 請求書を sats 建てにする方法

請求書の通貨単位として Satoshis を使うには、`BTC` の代わりに `SATS` を利用します。

または、`Store > Settings > Checkout Experience > Display Lightning payment amounts in Satoshis` オプションでも設定できます。

## 支払い後にストア請求書をリダイレクトするには？

ストアの支払い済み請求書を自動リダイレクトするには、`Stores > Settings > Checkout experience > Redirect invoice to redirect url automatically after paid` を有効化します。

この設定は、[Payment Button](../Apps.md#payment-button) のようにストアへ直接作成された請求書でよく使われます。支払い後、請求書は支払いボタンを埋め込んだ元ページ、または Payment Button 編集ページで指定したリダイレクト URL に戻ります。

この機能が無効な場合、顧客には請求書画面で元の支払いページへ戻る案内が表示されます。

![Redirect Paid Store Invoices](../img/invoice/PaidInvoice.png)

Point of Sale アプリで特定 URL にリダイレクトしたい場合は、代わりに [PoS Redirect](../FAQ/Apps.md#how-to-redirect-to-another-site-after-payment) を使用してください。

## BTCPay から請求書を削除できますか？

BTCPay Server の請求書は削除できませんが、アーカイブできます。
請求書をアーカイブするには、請求書一覧から対象を選び、アクションドロップダウンでアーカイブを選択します。もしくは請求書詳細ページ右上の `Archive` ボタンをクリックします。
この操作により、`Invoices` ページから表示が除外されます。

請求書は `Archived` ボタンをクリックするか、アーカイブ検索フィルターで表示して復元できます。詳細は [こちら](../Invoices.md#archived-invoices) を参照してください。

## 購入者の追加情報を収集するには？

請求書詳細ページの Buyer information セクションは、WooCommerce 連携や API での請求書作成など、カスタムソリューション向けです。現時点では BTCPayServer インターフェースで Buyer Information を収集する方法はありません。

## 請求書の為替レートプロバイダーを変更するには？

BTCPay 請求書で使用されるデフォルトの法定通貨-暗号資産レートプロバイダーは、`Store Settings > Rates > Preferred price source` で変更できます。複数のレートプロバイダーから選択でき、ストアごとに異なる設定が可能です。

## GetRatesAsync was called on coinaverage エラーについて

Coinaverage は無料ティア API の提供を終了しました。その結果、次のエラーが表示されることがあります。

```
GetRatesAsync was called on coinaverage when the rate is outdated. It should never happen, let BTCPayServer developers know about this.
```

この問題は、`Stores > Settings > Rates` で [別のレートソースプロバイダーを選択](./Stores.md#how-to-change-the-exchange-rate-provider-for-invoices) するか、`1.0.3.146` 以前を使っている場合は [BTCPay Server を更新](./ServerSettings.md#how-to-update-btcpay-server) することで解決できます。更新すると Coinaverage は自動的に CoinGecko に置き換えられます。

## payment request とは何ですか？

payment request を使うと、シンプルな支払いリンクを送るだけで顧客へ請求し、ビットコインで支払いを受け取れます。通常のビットコインウォレットだけで顧客に請求する場合、一般的な流れは次のようになります。

    Customer: 100$ をビットコインで払えますか？
    You: もちろんです。こちらが私の BTC アドレスで、送信時点で 100$ は 0.0025481 BTC です。
    Customer; メッセージに気づくのが遅れました。ウォレットでは今 0.0028 になっていて、追加で BTC を買う必要があります。明日払ってもいいですか？
    You: はい。ただ明日また同じやり取りが必要です。

BTCPay Server の組み込み [Payment Request](../PaymentRequests.md) 機能を使うと、顧客/クライアントとシンプルなリンクを共有するだけで支払いを受け取れます。
顧客には見やすく設計された payment request ページが表示され、都合の良いタイミングで支払えます。また、ビットコイン換算レートは常に選択通貨の最新値で表示されます。
支払い完了後、顧客は帳簿管理のために payment request を印刷できます。

[![BTCPay Server Payment Requests](https://img.youtube.com/vi/j6CvwDPvfzQ/mqdefault.jpg)](https://www.youtube.com/watch?v=j6CvwDPvfzQ)

この機能の詳細は、専用の [payment request ページ](../PaymentRequests.md) を参照してください。

## payment request と invoice の違いは何ですか？

invoice は、売り手が買い手から代金を回収するために発行する文書です。

BTCPay Server では、invoice は固定為替レートで定義された時間内に支払う必要がある文書を表します。**Invoice には有効期限があります**。これは受取側を価格変動から守るため、指定時間内でレートを固定するためです。

固定レートの invoice と異なり、payment request は「期限のない請求書」と考えることができます。支払者が生成した時点の最新レートが常に提供されます。
