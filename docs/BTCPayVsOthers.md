<a id="btcpay-server-vs-others"></a>

# BTCPay Server と他サービスの比較

![BTCPay Server vs other payment processors](./img/infographics/BTCPay-How-Is-It-Different.png)

多くの新規加盟店は、まずサービス料金に注目しがちです。**BTCPay Server は無料**なので、それがきっかけでここに来たのであれば歓迎します。

**BTCPay Server は企業ではなくコード**です。**加盟店と顧客の間に第三者は存在しません**。加盟店は常に資金を完全に管理できます。決済手数料やサブスクリプション料金はありません。BTCPay Server は無料で利用でき、完全なオープンソースなので、開発者やセキュリティ監査者は常にコード品質を検証できます。

私たちは、BTCPay Server だけでなく、加盟店に代わって決済がどのように処理されるかも理解してほしいと考えています。さまざまな**暗号通貨決済処理方式**のトレードオフを理解する助けになるためです。どの**決済プロセッサー**がどのサービスを提供しているかは私たちには分かりません。そこはご自身で調査する必要があります。以下の一覧は、その出発点として有用です。

- [BTCPay Server と他サービスの比較](#btcpay-server-vs-others)
  - [機能](#features)
  - [コスト](#cost)
  - [セキュリティ](#security)
  - [プライバシー](#privacy)
  - [検閲耐性](#censorship-resistance)
  - [分散性](#decentralized)
  - [法定通貨](#fiat)
  - [他の決済プロセッサーでこの情報が見つからない場合](#cant-find-this-information-for-other-payment-processors)

---

<a id="features"></a>

## 機能

どの決済プロセッサーにも機能はあります。以下は BTCPay Server の主な機能です。

- **無料 & ピアツーピア** - 直接の P2P 決済。加盟店向け処理手数料なし。トランザクション手数料なし（[ネットワーク手数料](https://en.bitcoin.it/wiki/Miner_fees)を除く）。
- **セルフホスト** - あなたのノード、あなたのコイン。仲介者なし。KYC/AML なし。ノンカストディアル（秘密鍵を完全管理）。[ハードウェアウォレット統合](./HardwareWalletIntegration.md)に対応。
- **Bitcoin & Altcoins** - Bitcoin をネイティブで受け入れ。必要に応じて [altcoin](./FAQ/Altcoin.md) 連携を追加可能。
- **先進機能** - ネイティブ Segwit 対応。Lightning Network（LND, Core Lightning (CLN), Eclair）で高速マイクロペイメント。
- **CMS 連携** - WordPress & WooCommerce、Shopify、Drupal、Magento、Prestashop、OpenCart、WHMCS などに対応し、カスタム連携も可能。
- **Apps** - 実店舗向け Point-Of-Sale 画面。寄付目標や資金調達向け Crowdfunding 画面。
- **Payment Buttons** - 埋め込みが簡単な HTML の寄付ボタン・支払いボタン。
- **ストア数無制限** - 加盟店は自店舗向けにも他者向けにも決済を処理可能。
- **翻訳** - 顧客は 20 以上の言語で支払い可能。
- **Payment Requests** - 商品やサービスの支払いを依頼する長期有効な請求書を作成・送信可能。
- **プライバシーとセキュリティ重視** - Payjoin 対応。Tor 対応。
- **BitPay 互換** - BitPay API と完全互換。BTCPay Server への移行が容易。

---

<a id="cost"></a>

## コスト

重要なのは、Bitcoin ネットワークでの支払いには、ブロックチェーンへ含めるためのトランザクション（マイナー）手数料が_常に_必要である点です。取引が承認されるか、いつ承認されるかは Bitcoin ネットワークが決定します。

**BTCPay Server は加盟店が顧客へ提示する直接支払い用の請求書を作成**し、さらに**ブロックチェーンを監視**して各支払い・寄付の承認状態を保存します。これを行うにはサーバー上での稼働が必要で、加盟店は自前ハードウェアでの運用、VPS の利用（$10/月未満）、または他者の BTCPay Server インスタンスでのアカウント利用（無料/有料）を選べます。

VPS で BTCPay Server を運用する場合、以下の種類の手数料は**一切発生しません**。

- 加盟店手数料
- サブスクリプション料金
- 送金手数料
- ソフトウェア利用料

---

<a id="security"></a>

## セキュリティ

Bitcoin の第一原則は、秘密鍵を常に _private_ のまま保つことです。新規加盟店には、秘密鍵の唯一の生成元（提供者）となる**安全なウォレット**の利用を推奨します。誰か（Web サイトなど）が秘密鍵を知っている・保存している・提供している可能性があるなら、一般的にはその鍵は実質的に秘密ではありません。

---

<a id="privacy"></a>

## プライバシー

BTCPay Server が加盟店に個人認証情報を求めることはありません。

通常、加盟店に代わって法定通貨へ/から換金する場合、決済プロセッサーには KYC（本人確認）および AML（マネーロンダリング対策）の銀行要件として個人情報の収集が求められます。これにはパスポート ID、電話番号、住所、銀行口座などが含まれる場合があります。

幸い、Bitcoin ネットワークはこの種の個人情報を利用・収集しません。そのため BTCPay Server も同様です。
**BTCPay Server がプライバシーを確保する方法**:

- 仲介者がいない
- 情報共有は顧客と販売者の間のみ
- セルフホスト利用者は [フルノードを運用][5]
- アドレス再利用なし
- Tor 対応
- [Payjoin](./Payjoin.md) 対応

---

<a id="censorship-resistance"></a>

## 検閲耐性

**BTCPay Server は検閲耐性を持ちます**。運用者本人以外は制御できません。単一障害点もありません。
BTCPay Server はユーザー自身のハードウェアで実行できます。

---

<a id="decentralized"></a>

## 分散性

多くの決済プロセッサーは「仲介者はいない」「資金は直接ウォレットに届く」「即時清算できる」と主張します。
しかし、次のような条件に当てはまる場合、そのプロセッサーは**仲介者**として動作している可能性が高いです。

- 加盟店が受け取るまでの待ち時間が、必要十分なブロックチェーン承認時間より長い。
- 決済プロセッサーが顧客支払いをまとめてから加盟店ウォレットへ送る。
- 加盟店の取引量に何らかの上限がある。
- 顧客ウォレットから送信後に、決済プロセッサーが支払いを拒否・却下・変更できる。
- 利用規約でアカウントの保留や凍結が可能と定めている。
- 決済プロセッサー利用料が、顧客から加盟店への支払いから自動控除される。

**決済プロセッサー**は**カストディアルウォレット**を使うことで仲介者として機能できます。顧客支払いを加盟店へ転送する前に、内部カストディアルウォレットで変更できるためです。これにより、手数料徴収、検証・処理のための支払い保留などが可能になります。この種のウォレットは、加盟店ウォレットと顧客ウォレットの間にある中間ウォレットです。

決済プロセッサーが加盟店向けにカストディアルウォレットを提供する場合もあります。前述のとおり、秘密鍵漏えいリスクがあるため推奨されません。「秘密鍵は渡した後に保存しない」と主張しても、真偽を知る頃には手遅れかもしれません。中央集権型サービスは簡単に見えますが、その代償として、Bitcoin ネットワークで本来得られるプライバシー、セキュリティ、自己主権を失う可能性があります。

---

だからこそ BTCPay Server は作られました。**加盟店が第三者依存を減らし、Bitcoin ネットワークを自由かつ安全に使えるようにするため**です。加盟店は BTCPay Server ソフトウェアの自分専用コピーを持ち、任意のサーバーまたは VPS 上で実行し、自分のノードで自分の決済を検証します。これはセルフホストのピアツーピア決済プロセッサーです。このアプローチのトレードオフとして、初期設定には一定の技術理解が必要です。

BTCPay Server コミュニティの成長に伴い、非技術ユーザーでも使いやすくなるよう、導入方法、ユースケース、チュートリアルは継続的に追加されています。BTCPay Server は完全なオープンソースです。誰でもコミュニティに参加し、改善、機能追加、ガイド作成などを提案・実装できます。フィードバックは常に歓迎されます。

---

<a id="fiat"></a>

## 法定通貨


bitcoin から各国通貨への変換、または各種 altcoin から Bitcoin への変換は、複数のサードパーティ BTCPay Server プラグインで利用できます。ニーズに合う解決策を [利用可能なプラグイン一覧](https://plugin-builder.btcpayserver.org/public/plugins) から探してください。

---

<a id="cant-find-this-information-for-other-payment-processors"></a>

## 他の決済プロセッサーでこの情報が見つからない場合

- それはバグではなく仕様かもしれません。
- これらの情報は加盟店が確認できるべきです。
- [Awesome Payment Processor List](https://github.com/alexk111/awesome-bitcoin-payment-processors) を確認してください。
- BTCPay Server についてさらに質問がある場合は、[公式ドキュメント][7]を参照してください。

[1]: https://github.com/bitcoin/bips/blob/master/bip-0021.mediawiki
[2]: https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki
[3]: https://github.com/bitcoin/bitcoin/pull/14451
[4]: https://mainnet.demo.btcpayserver.org/translate
[5]: https://en.bitcoin.it/wiki/Why_Your_Business_Should_Use_a_Full_Node_to_Accept_Bitcoin
[6]: https://howtoacceptcrypto.com/chart/
[7]: https://docs.btcpayserver.org/
