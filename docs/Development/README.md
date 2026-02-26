# アーキテクチャ

BTCPay Server は、独自の決済プロセッサを導入・運用するために、**複数の Bitcoin 関連コンポーネント**を一貫したユーザー体験として統合するプロジェクトです。

![Architecture](../img/Architecture.png)

最小構成は次のとおりです。

- [BTCPay Server](https://github.com/btcpayserver/btcpayserver)
- [NBXplorer](https://github.com/dgarage/NBXplorer)（軽量ブロックエクスプローラー。支払い追跡を担当）
- Bitcoin Core
- PostgreSQL

さらに、Lightning Network へのアクセスが必要な場合、NBXplorer は次への接続をサポートします。

- Core Lightning (CLN)（unix ソケット経由）
- LND（REST インターフェース経由）

以下の動画では、**BTCPay アーキテクチャ**を詳しく解説しています。

[![BTCPay Architecture](../img/btcpay-architecture-advancing-bitcoin.png)](https://www.youtube.com/watch?v=Up0dvorzSNM)

---

BTCPay Server のデプロイ方法は、柔軟性重視か使いやすさ重視かに応じて複数用意されています。

簡単な順に並べると次のとおりです。

- [Web インターフェース（LunaNode）デプロイ](/Deployment/LunaNode.md)
- [Azure デプロイ](/Deployment/Azure.md)（Microsoft Azure のワンクリックデプロイを使用）
- [Docker デプロイ](https://docs.btcpayserver.org/Docker/)（ほぼあらゆる環境で、依存関係をまとめた `docker-compose.yml` ファイルを使用）
- [手動デプロイ](/Deployment/ManualDeployment.md)（すべての依存関係を自分でダウンロード・ビルド・実行）

コミュニティメンバーの一部は、[サードパーティホスティング](/Deployment/ThirdPartyHosting.md)（第三者による BTCPay Server 管理）も提供しています。

ウォレットと Web サービスを**直接コントロール**できることには**大きな価値**があります。そのため、[Azure デプロイ](/Deployment/Azure.md) または [Web インターフェースデプロイ](/Deployment/LunaNode.md) を利用し、**自分でセットアップすること**をおすすめします。手順はそれほど難しくありません。
