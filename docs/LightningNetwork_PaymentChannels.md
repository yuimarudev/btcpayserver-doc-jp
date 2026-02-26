# 決済チャネルの開設と運用

Lightning Network は現在も活発に開発が進む比較的新しい技術であるため、新しくデプロイしたノードを送受金できる状態にするには、いくつかの手順が必要です。

概要:

1. ライトニングノードをデプロイして有効化し、オンチェーンウォレットに資金を入れる
2. ピアを特定し、最初の決済チャネルを開設する
3. インバウンド流動性とアウトバウンド流動性を確保する。これでノードは **送金** と **受取** が可能になる
4. 流動性管理を継続する。**送金** と **受取** の能力を維持するための継続的な作業

重要な考慮点:

- **チャネル相手の選定**。最初のチャネルは、接続性が高く稼働率の高いピアに開くことを検討してください。支払いがルーティングされ、決済される可能性が高まります。
- **インバウンド** と **アウトバウンド** の容量。アウトバウンド容量はノードが支払いを **送金** するために必要で、インバウンド容量は支払いを **受取** するために必要です。ライトニングを使う加盟店にとって、顧客が支払えるようにするにはインバウンド容量が不可欠です。
- **インバウンド容量**。ノードは、ローカル残高から sats を使うか、ネットワーク内の他ノードにチャネルを開いてもらうことで、インバウンド容量を増やせます。
- **流動性管理**: 送受金能力を維持するには継続的な調整が必要であり、決済チャネル全体でインバウンド容量とアウトバウンド容量のバランスを保つ必要があります。この容量配分は、ノード運用者のユースケースに応じて調整する必要があります。
- **Lightning Service Providers**: LSP は、ライトニングノード運用を容易にする有償のサードパーティサービスを提供します。これらのサービスは、インバウンド容量の確保やリバランス自動化に利用できます。

以下は、次のようなトピックをより深く理解するための参考リソースです:

- [LN における良いピア](https://docs.lightning.engineering/the-lightning-network/the-gossip-network/identify-good-peers)
- [Lightning ノードの種類](https://bitcoin.design/guide/how-it-works/nodes/#lightning-nodes)
- [Lightning の流動性とは？](https://bitcoin.design/guide/how-it-works/liquidity/)
- [インバウンド容量を確保するには？](https://lightningnetwork.plus/posts/234)
- [流動性を管理するには？](https://docs.lightning.engineering/the-lightning-network/liquidity/manage-liquidity#rebalancing-channels)
- [Lightning サービスプロバイダー (LSP)](https://bitcoin.design/guide/how-it-works/lightning-services/)
