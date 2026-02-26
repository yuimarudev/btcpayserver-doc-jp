# BTCPay Server Payjoin ガイド

このドキュメントでは、BTCPay Server の **Payjoin** 機能の使い方を説明します。payjoin の実装に関する詳細で技術的な説明は [BIP78](https://github.com/bitcoin/bips/blob/master/bip-0078.mediawiki) を参照してください。

以下の動画で、payjoin の概要と使い方を理解できます。

[![BTCPay Server で Payjoin を使う方法](https://img.youtube.com/vi/-Wrqv6nSmAM/mqdefault.jpg)](https://www.youtube.com/watch?v=-Wrqv6nSmAM)

## 加盟店として Payjoin を有効にする

1. ストアを作成する
2. 派生スキーム用の [hot wallet](./CreateWallet.md#hot-wallet) を設定します。アドレスタイプは segwit または segwit wrapped を使用してください。
3. 「General Settings」で Payjoin/P2EP を有効化し、「Save」をクリックします

Payjoin を機能させるには、少なくとも 1 つの UTXO が必要である点に注意してください。

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Receive_1.png)

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Receive_2.png)

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Receive_3.png)

## ユーザーとして Payjoin で支払う

[BTCPay Wallet](./Wallet.md) は Payjoin に対応しています。

1. Payjoin が有効な BTCPay Server 請求書から BIP21 支払いリンクを取得します。方法は次のいずれかです:
   - カメラスキャン機能で QR コードを読み取る
   - 「Open in wallet」ボタンからリンクをコピーし、「Parse BIP21」プロンプトに貼り付ける
2. 送金フォームに支払い情報が自動入力されるはずです。請求書が payjoin に対応しているかは、「advanced settings」を展開し、URL が入った「Payjoin endpoint」入力欄があるかで確認できます。
3. [BTCPay Vault](./HardwareWalletIntegration.md) 経由の BTCPay Server ハードウェアウォレット対応、または [hot wallet](./CreateWallet.md#hot-wallet) 機能を使ってトランザクションに署名します。
4. 元のトランザクションの準備ができると、`Broadcast (Payjoin)` または `Broadcast (Simple)` を選べます。`Broadcast (Payjoin)` を選択してください。
5. 可能な場合、payjoin サーバーが新しい特別なトランザクションを提案します。payjoin を実行できない場合は、代わりに元のトランザクションがブロードキャストされます。
6. ハードウェアウォレットを使用している場合は、payjoin トランザクションへの再署名を求められます（hot wallet 機能では自動署名されます）。
7. これで、Bitcoin の代替可能性と金融主権の向上に貢献できました。

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Pay_1.png)

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Pay_2.png)

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Pay_3.png)

![BTCPay Server で PayJoin を受け取る](./img/payjoin/Payjoin_Guide_Pay_4.png)

## なぜ payjoin が発生しなかったのですか？

理由はいくつかあります:

- ストア側に payjoin に使える utxos がなかった
- あなたのウォレットがストアと同じフォーマットを使っていない（分析企業に疑いを持たれにくくするために重要）
- segwit または p2sh wrapped segwit を使っていない
- payjoin サーバーが利用できない

## 対応ウォレット

対応追加をウォレット開発者に依頼し、後押ししてください。**payjoin の利用** が広がるほど、ブロックチェーン分析企業のヒューリスティクスは崩れ、金融履歴の追跡は難しくなります。ウォレット開発者の方で、支援やフィードバックが必要な場合は [contact us](./Community.md) からご連絡ください。
