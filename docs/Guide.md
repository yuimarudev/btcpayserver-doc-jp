# BTCPay Server ドキュメント

## BTCPay Server とは？

BTCPay Server は、無料・オープンソース・セルフホスト型の Bitcoin 決済ゲートウェイです。個人や事業者が、手数料なしでオンラインまたは対面で Bitcoin 決済を受け取れるようにします。

![BTCPay Server](./img/BTCPayServerScreenshot.png 'BTCPay Server screenshot')

## BTCPay Server はどのように動作する？

BTCPay Server はセルフホスト型の自動請求システムです。チェックアウト時に顧客へ請求書が提示され、顧客は自分のウォレットから支払います。BTCPay Server はブロックチェーン上で請求書状態を追跡し、決済完了時に通知するため、注文処理を進められます。さらに返金処理や Bitcoin 管理など、多くの機能も備えています。

[![How BTCPay Works](https://img.youtube.com/vi/nr0UNbz3AoQ/mqdefault.jpg)](https://www.youtube.com/watch?v=nr0UNbz3AoQ)

[![BTCPay Server Simply Explained](https://img.youtube.com/vi/dbX6qWZlxOw/mqdefault.jpg)](https://www.youtube.com/watch?v=dbX6qWZlxOw)

BTCPay Server は無料で利用でき、完全なオープンソースです。そのため開発者やセキュリティ監査担当者は、いつでもコード品質を検証できます。

## 機能

- Bitcoin の直接 P2P 決済
- 取引手数料なし（[ネットワーク手数料](https://en.bitcoin.it/wiki/Miner_fees)を除く）
- 処理手数料なし
- 仲介業者なし
- KYC なし
- ノンカストディアル（秘密鍵を完全に自分で管理）
- プライバシー向上
- セキュリティ向上
- セルフホスト型ソフトウェア
- SegWit 対応
- Lightning Network 対応（LND、Core Lightning (CLN)、Eclair 実装）
- Tor 対応
- オプトインの [altcoin](./Development/Altcoins.md) 連携
- 従来 BitPay API との完全互換（容易な移行）
- 他者向け決済処理
- 埋め込みやすい決済ボタン
- POS アプリ
- クラウドファンディングアプリ
- 支払いリクエスト
- [ハードウェアウォレット連携](./HardwareWalletIntegration.md) に対応した内部フルノード依存ウォレット
- [Payjoin サポート](./Payjoin.md)

## はじめに

BTCPay Server の利用を始めるには、まず [導入方法を選択](/Deployment/) します。セルフホストを選ぶ場合は、まず導入ドキュメントを確認してください。推奨は [Docker デプロイ](/Docker/) です。サードパーティホスティングを選ぶ場合は [サードパーティホスト向けドキュメント](/Deployment/ThirdPartyHosting.md) を参照してください。

[![How BTCPay Server Features Overview](https://img.youtube.com/vi/R-yaXk4NvEs/mqdefault.jpg)](https://www.youtube.com/watch?v=R-yaXk4NvEs)

## 参加する

オープンソースプロジェクトへの貢献は、学習・ネットワーク形成・実績作りに非常に有効です。BTCPay Server はインターネット上のボランティアにより維持されています。開発に貢献したい場合は [貢献ガイドライン](/Contribute/) を確認してください。

ドキュメント支援に関心がある場合は、以下の動画も参照してください。

[![Contributing to Documentation](https://img.youtube.com/vi/bSDROcdSSWw/mqdefault.jpg)](https://www.youtube.com/watch?v=bSDROcdSSWw)

## サポート

BTCPay Server の利用で問題がある場合は、[公式サイトに掲載されているコミュニティ](https://btcpayserver.org/#communityCTA) で BTCPay コミュニティメンバーに相談することを検討してください。

他チャネルで解決できない技術的問題や、コミュニティメンバーと検証済みの機能要望についてのみ [GitHub issue](https://github.com/btcpayserver/btcpayserver/issues) を作成してください。

詳細は [公式サイト](https://btcpayserver.org/) と [FAQ](./FAQ/README.md#btcpay-frequently-asked-questions-and-common-issues) を参照してください。

!!!include(supporters.html)!!!
