---
tags:
  - refund
  - merchant refund
---
<!-- legacy-anchor-aliases -->
<span id="do-i-need-to-have-an-online-store-to-use-btcpay-server"></span>
<span id="does-btcpay-need-myprivate-key"></span>
<span id="does-btcpay-server-support-crypto-to-fiat-conversion"></span>
<span id="how-can-i-backup-my-btcpay-server"></span>
<span id="how-can-i-charge-for-using-my-btcpay-server-instance"></span>
<span id="how-can-i-contribute-to-btcpay"></span>
<span id="how-can-i-use-the-btcpay-server-api"></span>
<span id="how-does-btcpay-create-a-new-address-for-each-invoice"></span>
<span id="how-to-install-btcpay-server"></span>
<span id="what-if-i-have-a-problem-paying-an-invoice"></span>
<span id="what-if-i-have-a-problem-with-a-paid-invoice"></span>
<span id="what-is-btcpay-server"></span>
<span id="where-can-i-get-help-and-support"></span>
<span id="where-to-find-btcpay-video-tutorials"></span>
<span id="who-can-use-btcpay"></span>
<span id="why-cant-i-just-give-my-bitcoin-address-to-a-buyer"></span>
<span id="why-is-everyone-so-excited-about-btcpay"></span>
<span id="why-should-i-choose-btcpay-over-other-processors"></span>
<!-- /legacy-anchor-aliases -->


# 一般 FAQ

このページには、BTCPay Server に関する一般的な質問と回答をまとめています。何であるか、どのように動作するか、どのようにインストールするかを扱います。

[[toc]]

## BTCPay Server とは何ですか？

BTCPay Server は、無料かつオープンソースの暗号資産決済プロセッサです。仲介者なし、手数料なし、取引コストなしで、Bitcoin（オンチェーンおよび Lightning Network）とアルトコインの支払いを直接受け取れます。

BTCPay はノンカストディアルな請求システムであり、第三者の関与を排除します。BTCPay の支払いはあなたのウォレットに直接届くため、プライバシーとセキュリティが向上します。BTCPay Server で支払いを受け取るために秘密鍵は不要です。各請求書で新しい受取アドレスを使用するため、[アドレスの再利用はありません](#how-does-btcpay-create-a-new-address-for-each-invoice)。

## 他の決済プロセッサではなく BTCPay を選ぶべき理由は？

BTCPay の最も大きな利点は、コミュニティによって作られた、完全に無料でオープンソースかつノンカストディアルなソフトウェアであることです。多くの他社プロセッサがあなたの Bitcoin を保管する一方で、BTCPay ではソフトウェアウォレットまたはハードウェアウォレットへ P2P で直接支払いを受け取れます。

BTCPay はセルフホスト型ソフトウェアです。つまり、あなた自身が決済プロセッサになります。サブスクリプションは不要で、取引手数料もありません。第三者の関与がないため、あなたと顧客の検閲耐性・プライバシー・セキュリティが大きく向上します。さらに BTCPay により、あなた自身がプロセッサとして異なるプランを提供し、ローカルまたはグローバルで普及を後押しできます。

BTCPay では、あなた自身が銀行です。

## なぜ BTCPay にこれほど注目が集まっているのですか？

コミュニティが BTCPay に注目し、加盟店やコンテンツクリエイターに推奨する理由は、店舗オーナーや慈善団体が Bitcoin 支払いを直接受け取れるためであり、顧客や寄付者のプライバシーが大幅に向上するからです。

BTCPay は Bitcoin の主要な特性のひとつである検閲耐性を損ないません。さらに無料かつオープンソースであることから、開発者にとって BTCPay 上で開発や連携を行う優れた機会になります。

## BTCPay は誰が使えますか？

BTCPay Server は機能が豊富で、さまざまな利用者タイプの課題を解決できる多くのユースケースがあります。加盟店、コンテンツクリエイター、Lightning Network ユーザー、取引所、ホスティング事業者など、多くの人に有用です。詳細は [ユースケースページ](../UseCase.md) を参照してください。

BTCPay は [MIT License](https://github.com/btcpayserver/btcpayserver/blob/master/LICENSE) の下で提供されています。

## BTCPay Server をインストールするには？

まず、さまざまなデプロイオプションを確認し、自分の要件に最も合うものを検討してください。

- [すべてのデプロイ方法を見る](/Deployment/README.md)

まだ質問がある場合は、[Deployment FAQ](/FAQ/Deployment.md) を参照してください。

## BTCPay の動画チュートリアルはどこで見つかりますか？

BTCPay Server の解説動画は、公式 BTCPay Server YouTube チャンネルで確認できます。

- [BTCPay YouTube チャンネル](https://www.youtube.com/channel/UCpG9WL6TJuoNfFVkaDMp9ug/videos)
- [BTCPay の YouTube 動画をまとめたプレイリスト](https://www.youtube.com/playlist?list=PL7b9Wt9shK2r-WXS6ysG4tafVQRu80biZ)

## BTCPay Server を使うにはオンラインストアが必要ですか？

eコマースストアがなくても BTCPay は利用できます。BTCPay Server を立ち上げて、友人や地域マーケット向けの決済プロセッサとして運用できます。別のユースケースとして、POS（Point of Sale）アプリや支払いボタンを使って寄付を受け付けることもできます。これらは HTML スニペットとして任意のウェブサイトにコピー＆ペースト可能です。

## なぜ購入者に自分の Bitcoin アドレスを渡すだけではだめなのですか？

受取アドレスの再利用はプライバシー上の問題です。顧客ごとに異なるアドレスを手動で渡すのは最適ではありません。暗号資産で支払いたい全員に、毎回一意のメールを送る状況を想像してください。

BTCPay はアドレス再利用の問題を解決します。顧客が BTCPay で支払うたびに、加盟店のウォレットから作成した一意アドレス付きの新しい請求書を生成し、チェックアウト処理を自動化します。eコマース連携を使っている場合、BTCPay Server はチェックアウトに組み込まれ、顧客は通常の決済手段と同じように数クリックで Bitcoin やアルトコインで支払えます。

顧客が支払いを行うと、BTCPay Server は注文が支払い済み／完了であることをストアに通知します。利用している eコマースソフトウェアによっては注文ステータスも変更されます。あなたは商品の発送に集中し、請求と決済処理は BTCPay に任せられます。

## BTCPay は請求書ごとに新しいアドレスをどのように作成しますか？

BTCPay Server には、既知のプライバシー問題であるアドレス再利用を解消する重要な機能があります。支払い用の請求書が要求されるたびに新しいアドレスを提供することで実現します。これはすべて自動で行われ、加盟店がどのアドレスがどのウォレットやストアに属するかを管理する必要はありません。BTCPay Server は支払い情報を、加盟店向けの詳細な請求システムで整理します。

仕組みは比較的シンプルです。加盟店は支払いを受けたい各ストアにウォレットを接続します。ストア支払い用に発行される請求書は、加盟店が接続したウォレットに直接紐づきます。請求書アドレスは、ストアに関連付けられたウォレットの [xpubkey](https://bitcointalk.org/index.php?topic=2828777.0) から導出されます。ソフトウェアが新しい受取アドレスを生成するために必要なのは、ウォレットの拡張公開鍵のみです。これらのアドレスは、ブロックチェーン上での移動を BTCPay Server が監視します。各ストアの請求書ページには、これらアドレスへの支払い状況が詳細に表示されます。

## BTCPay は秘密鍵を必要としますか？

既存ウォレットで BTCPay を使う場合、秘密鍵は不要です。オンチェーン取引のためにマスター秘密鍵へのアクセスを BTCPay Server が要求しないことは、大きなセキュリティ上の利点です。仮にサーバーが侵害されても、オンチェーン取引の資金は常に安全です。オンチェーン資金の保護は、最終的には [ウォレットの保護](https://btcinformation.org/en/secure-your-wallet) にかかっています。 [既存ウォレットを BTCPay Server で使う](../WalletSetup.md#use-an-existing-wallet) 場合は、ウォレットの公開鍵のみが必要です。

BTCPay Server で新しいウォレットを生成することも可能で、これはサーバーに保管されるホットウォレットになります。Lightning ノードがある場合、BTCPay は技術的には Lightning 資金の鍵（macaroons）にもアクセスできます。これらの機能を使いたい場合は、実験的機能に伴う [セキュリティ上の影響とリスク](../CreateWallet.md#security-implications) を十分に理解してください。

Third-Party BTCPay ホストを利用している場合は、秘密鍵に関する [セキュリティ上の懸念](../Deployment/ThirdPartyHosting.md#security-concerns) を理解しておく必要があります。

## BTCPay Server は暗号資産から法定通貨への換算に対応していますか？

法定通貨換算は、さまざまな BTCPay Server の [plugins](https://plugin-builder.btcpayserver.org/public/plugins) を通じて利用できます。利用要件に合うソリューションを、利用可能なプラグインから探してください。

## 請求書の支払いで問題がある場合は？

BTCPay Server の請求書支払いで問題が発生する場合、主に以下の原因が考えられます。

1. SegWit 非対応ウォレットで支払おうとしており、加盟店の請求書が Bech32 形式を使っている。

これは比較的よくある問題ですが、支払い時に `invalid address` のようなエラーが表示されるため、利用者には分かりにくい場合があります。この場合（購入者側）の解決策は、Bech32 アドレス送金をサポートする [SegWit 対応ウォレット](https://en.bitcoin.it/wiki/Bech32_adoption) を使うことです。

この場合（加盟店側）の解決策は、BTCPay Server ストアで設定する拡張公開鍵（xPub）を変更することです。具体的には、xPub の末尾に `-[p2sh]` を付けると、請求書アドレスが自動的に変更され、SegWit／非 SegWit の両ウォレットが支払えるようになります。BTCPay Server ウォレットは、xPub のアドレスを Pay to Script Hash（p2sh）でラップすることで、より広く受け入れられるアドレスを生成します。この対策を導入する前後でウォレットや受取支払いにどのような影響があるかを理解することが重要です。ストアの xPub を変更すると、BTCPay Server ストアの観点では完全に新しいウォレットが生成されます。以下を理解したうえで実施してください。

- BTCPay Server が生成したホットウォレットを使っている場合、xpub を変更しても新しいシードワードは作成されず、以前のホットウォレットのシードワードはサーバーに **保存されなくなります**。
  - その結果、新しい資金を使えなくなります。代わりに新しいストアと新しい BTCPay Server ホットウォレットを作成し、`Segwit wrapped (Compatible with old wallets)` のアドレスタイプを選択して、新しいストアのウォレットへ資金を移行してください。）
- 別のウォレット（ハードウェア／ソフトウェアウォレットなど）から xPub をインポートした場合、xPub 変更後は外部ウォレットが支払いを検出できなくなります。
  - その結果、Hardware Wallet Integration（Vault、推奨）またはシード署名（非推奨）を使い、BTCPay Server 内部ウォレット経由で資金を使うことになります。
- 以前ストアウォレットに表示されていた過去の資金や取引は表示されなくなります。
  - その結果、変更後 xpub を使う2つ目のストアを作成し、過去の取引履歴を保持することを検討するとよいでしょう。

xpub 形式と変更方法の詳細は [こちら](./Wallet.md#what-is-a-derivation-scheme) を参照してください。上記オプションが不明な場合は、[Mattermost コミュニティ](https://chat.btcpayserver.org/) で確認してください。

2. 請求書への支払いは受信しているが、全額が支払われていない。

取引所や他のカストディアルサービスから支払う場合、支払い額の一部が手数料として差し引かれることがあります。解決策は、不足分を支払う（請求書が期限切れでない場合）か、返金または不足分の支払い方法について加盟店へ連絡することです。

## 支払い済み請求書で問題がある場合は？

:::tip
加盟店に返金を依頼するには、加盟店へ直接連絡してください。BTCPay Server は、商品やサービスを購入した加盟店とは関係がありません。
:::

BTCPay Server はオープンソースのセルフホスト型ソフトウェアスタックであり、企業ではありません。BTCPay Server のコミュニティとコントリビューターは、誰がソフトウェアを使うか、どのように使うかを制御できません。
加盟店に請求書を支払い、注文に問題がある場合は、何が起きたのかを確認するため加盟店へ直接連絡する必要があります。

ソフトウェアを運用する各加盟店は、自分のストアと受取ウォレットを自ら管理します。BTCPay Server コミュニティは BTCPay Server を使うストア資金を保持せず、アクセスもできません。アクセスできるのは加盟店のみです。

## どこでヘルプやサポートを受けられますか？

BTCPay はオープンソースプロジェクトです。企業ではないため、メール・ライブチャット・電話サポートはありません。ソフトウェアのサポートは、コントリビューターとユーザーのネットワークによって提供されています。

問題に遭遇したり機能要望がある場合は、[GitHub で issue を作成](https://github.com/btcpayserver/btcpayserver/issues) してください。一般的な質問については、[Mattermost コミュニティ](https://chat.btcpayserver.org/) に参加してください。一部コミュニティメンバーは [有償サポート](../Support.md) を提供しています。

## BTCPay に貢献するには？

BTCPay のようなオープンソースプロジェクトへの貢献方法は数多くあります。

最も簡単なのは、ソフトウェアを使い、フィードバックを提供し、あなたや顧客が遭遇したバグや問題を報告することです。開発者であれば、BTCPay Server の [GitHub リポジトリ](https://github.com/btcpayserver) でのコントリビュートを通じて開発・改善に参加できます。[Transifex](https://www.transifex.com/btcpayserver/btcpayserver/) で BTCPay を母国語へ翻訳すること、ドキュメントや執筆を手伝うことも、開発者や技術者でなくてもできる貢献です。すべてのコントリビューターに感謝しています。

貢献方法の全体像は [contribute セクション](../Contribute/README.md) を確認してください。

## BTCPay Server API はどのように利用できますか？

従来の BTCPay Server API は、主に [BitPay の API](https://bitpay.com/api/) と互換性があり、無料かつオープンソースの決済処理代替として BTCPay へ移行しやすくなっています。

2020年に BTCPay Server は新しい Greenfield API の提供を開始しました。この API は従来 API と共存し、UI を使わずに BTCPay Server の全機能を利用できるようにします。現在の [Greenfield API ドキュメント](https://docs.btcpayserver.org/API/Greenfield/v1/) を参照してください。

Greenfield API ドキュメントに存在しない BTCPay Server 機能は、新 API でまだ完全実装されていないことを意味します。その場合は従来 API を利用してください。新しい Greenfield API の開発議論は [こちら](https://github.com/btcpayserver/btcpayserver/issues/1320) で確認できます。

## webhook を作成するには？

BTCPay Server では、新しい `Webhook` の作成は比較的簡単です。
BTCPay Server ダッシュボードで `Store settings` に移動し、`Webhooks` タブをクリックします。

![新しい Webhook の作成](../img/FAQ/btcpayWebhookFAQ1.jpg)

`Webhook` 作成画面が開きます。
`Payload` URL を把握したうえで、BTCPay Server に貼り付けてください。
`payload` URL を貼り付けると、その下に `webhook` secret が表示されます。

`webhook` secret をコピーし、エンドポイント側に設定します。
設定が完了したら、BTCPay Server で `Automatical redelivery` を切り替えられます。
配信に失敗した場合は、10秒後、1分後、その後10分ごとに最大6回まで再配信を試みます。
すべてのイベントを対象にするか、必要なイベントだけを指定できます。

webhook を有効化し、`Add webhook` をクリックして保存してください。

![新しい Webhook の作成](../img/FAQ/btcpayWebhookFAQ2.jpg)

## Webhook 形式は BitPay 互換ではないのですか？

Webhooks は bitpay API との互換を目的としていません。
BTCPay Server には、2種類の IPN（BitPay 用語では「Instant Payment Notifications」）があります。

- Webhooks
- notifications

`Webhooks` は Greenfield Events で、`Notifications` は Bitpay events です。
Bitpay 経由で請求書を作成する場合は `Notification URL` を使用してください。

この質問の詳細は [Source](https://github.com/btcpayserver/btcpayserver/discussions/2282) を参照してください。

[Greenfield API ](https://docs.btcpayserver.org/API/Greenfield/v1/) の詳細はこちらです。

PHP で `Webhook` を処理する方法は、次の [example script](https://github.com/btcpayserver/btcpayserver-greenfield-php/blob/master/examples/webhook.php) を参照してください。

## BTCPay Server をバックアップするには？

BTCPay Server インスタンスとそのデータは [バックアップ作成](https://docs.btcpayserver.org/Docker/backup-restore/) が可能です。ただしバックアップスクリプトは、すべての BTCPay Server 構成やカスタムデプロイで十分に検証されているわけではありません。本番運用で依存する前に、バックアップから正しく環境を再現できることを必ず確認してください。

## BTCPay Server インスタンスの利用に対して課金できますか？

現在、取引割合での課金や登録料など、BTCPay Server インスタンス利用者に課金する機能はネイティブではサポートされていません。
[Greenfield API](https://docs.btcpayserver.org/API/Greenfield/v1/) を使えば実現できる可能性はありますが、中程度から高度な技術知識が必要です。

## 同期が止まる: 「NBXplorer is synchronizing」

場合によっては NBXplorer が停止したように見えることがあります。まず最初に試すべきは更新です。Docker デプロイを使っている場合は、`./btcpay-update.sh` を実行するか、`Server settings / Maintenance / Update` に進んでください。

再起動しても問題が続き NBXplorer が停止したままの場合、以下のように同期ダイアログが表示され、高さ（height）がこのスクリーンショットのように変化しないことがあります。

![NBXplorer-stuck](./../img/NBXplorer-stuck.png)

この問題は、サーバーが長期間オフラインだった場合に発生しやすく、かつ BTCPay Server Docker デプロイのデフォルト設定である pruned な Bitcoin フルノードを使っていると起こります。

サーバー再起動後、Bitcoin フルノードの同期が完了してから NBXplorer が同期を開始します。しかしフルノード同期後に、NBXplorer が必要とするブロックが prune されてしまう場合があります。

この状況を解消する唯一の方法は、NBXplorer に問題のブロックをスキップさせることです。これにより、その期間に発生した取引は表示できなくなりますが、BTCPay Server 自体はオンラインに復帰します。

```bash
docker stop generated_nbxplorer_1

docker exec -ti generated_postgres_1 psql -U postgres -d nbxplorermainnet -c "DELETE FROM nbxv1_settings WHERE code='BTC' AND key='BlockLocator-';"

docker start generated_nbxplorer_1
```

サーバーは同期済みとなり、利用可能になるはずです。
