# BTCPay Server のコーディング

[[toc]]

## なぜコーディングが重要か

プロジェクトの成長、成熟、高度化、発展を支えるうえで、BTCPay Server に取り組むコーダーは中核的な存在です。

コードを書く、コードレビューするなどのスキルがあれば、BTCPay Server に貢献できます。

## どこから始めるか

開発者として貢献したいものの何から始めればよいか分からない場合は、[good first issue label](https://github.com/btcpayserver/btcpayserver/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) を確認してください。新しいコントリビューター向けの小さな作業がまとめられています。

もう少し難しい課題に取り組みたい場合は、プルリクエストを作成する前に、[issue を作成](https://github.com/btcpayserver/btcpayserver/issues/new/choose)するか、[コミュニティチャット](https://chat.btcpayserver.org/)に参加してください。早い段階でフィードバックを得て、最適な進め方を議論し、作業の重複を避けるためです。

私たちは、GitHub の issue を引き受けて解決し、開発を支援してくれる開発者を積極的に探しています。協力したいがガイダンスが必要な場合は、[#dev channel on Mattermost](https://chat.btcpayserver.org/btcpayserver/channels/dev) で質問してください。

### 開発環境のセットアップ

BTCPay Server の開発者またはテスターとして始めたい場合は、[Setup Developer Environment](./DevCode.md) ガイドを確認してください。Git、GitBash、Github、Docker、Visual Studio、Postgres など、BTCPay 開発で使うソフトウェアをステップごとに説明しています。コーディングが初めてで新しく学びたい場合も、ここから始めるのがおすすめです。

### ローカル BTCPay 開発

すでに開発環境のセットアップが完了している場合は、BTCPay 固有の [Local Development](/Development/LocalDevelopment.md) ドキュメントから始められます。

### ローカル BTCPay テスト

開発環境ツールのセットアップが完了し、ローカル BTCPay Server が動作しているなら、[Local Testing](./DevTest.md) ガイドを確認してください。開発用途や、リリース前に新機能を試したいユーザー向けに、regtest モードで BTCPay を使う方法を説明しています。

## 要件

ソフトウェア要件（IDE など）は [local development](/Development/LocalDevelopment.md#which-ide) にも記載されています。

## 動画

BTCPay Server 開発動画は [こちら](/Development/LocalDevelopment.md#videos) または [BTCPayServer YouTube](https://www.youtube.com/channel/UCpG9WL6TJuoNfFVkaDMp9ug) チャンネルで確認できます。
