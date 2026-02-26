# BTCPay Server のマルチシグ対応

BTCPay Server v2.1.0 では、セキュリティ、共同運用、柔軟性をさらに高めるために設計されたマルチシグネチャ（multisig）ウォレットのサポートが強化されました。

このドキュメントでは、BTCPay Server インスタンス内でマルチシグウォレットを設定して利用する手順を、ステップごとに説明します。

[![XPUB Extractor を使った BTCPay Server の強化マルチシグ](https://img.youtube.com/vi/V95pLvVTAqM/mqdefault.jpg)](https://www.youtube.com/watch?v=V95pLvVTAqM)

## 概要

BTCPay Server のマルチシグ機能により、次のことが可能になります。

- **共同保管**: トランザクションのブロードキャスト承認に複数の署名を必要とするウォレットを作成できます。
- **セルフホスト型マルチシグ調整**: 企業の公開鍵管理と署名フローを、第三者サービスに依存せず共有管理できます。
- **ハードウェアウォレット対応**: BTCPay Vault とシームレスに連携し、多数のハードウェア署名デバイスと互換性があります。ハードウェアウォレットでトランザクションを承認できます。
- **通知システム**: トランザクションが作成されたとき、保留中（必要署名数が不足）になったとき、ブロードキャストされたときにメール通知を受け取れます。
- **プラグイン連携**: Vendor Pay や今後の機能と連携し、マルチシグベースの payout を実現します。Xpub Extractor との連携により、マルチシグ参加者全員が適切な形式の情報を簡単に取得できます。

## 前提条件

次のコンポーネントがインストールおよび設定されていることを確認してください。

- BTCPay Server インスタンス（v2.1 以降）
- [BTCPay Vault](https://github.com/btcpayserver/BTCPayServer.Vault)
- [XpubExtractor Plugin](https://github.com/btcpayserver/BTCPayServer.Plugins.XpubExtractor)（ハードウェアウォレットをセットアップする場合）

## Step 1: 拡張公開鍵を収集する

以下では [hardware wallet](https://docs.btcpayserver.org/HardwareWalletIntegration/) から必要な公開鍵を取得する方法を説明します。ソフトウェアウォレットを使用する場合は Step 2 に進んでください。

1. `Manage Plugins` に移動し、**XpubExtractor** がインストール済みで、公開元が *RockstarDev* であることを確認します。
2. ハードウェアウォレットを接続し、[BTCPay Vault](http://docs.btcpayserver.org/HardwareWalletIntegration/) を起動します。
3. `Fetch Xpub` 機能を使って次の情報を取得します。
   - 拡張公開鍵（xpub）
   - Root fingerprint
   - Derivation path
4. ウォレット用のデータを保存します。

値の例:

```
xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKp5KUMRmD9YgoWDbEVpLFgje71pRAVBPX6DCmV9HNTLr8GHqKZANvNcFpSZe3kiKsH5Ej7ApG1NVDK
Root Fingerprint: abcdef21
Key Path: 48'/0'/0'/0'
```
### 追加署名者の招待

1. `Settings` > `Users` に移動します。
2. 各参加者のメールアドレスを追加し、生成された招待リンクを直接共有します。サーバーで Email SMTP を設定している場合、参加者には招待メールが送信されます。
3. 参加者には次を実施してもらいます。
   - 招待を承諾する
   - [BTCPay Server Store を作成する](https://docs.btcpayserver.org/CreateStore/)
   - [XpubExtractor](https://github.com/rockstardev/BTCPayServerPlugins.RockstarDev/tree/1235799827c24d33bfe1095db5169afd39e620f1/Plugins/BTCPayServer.RockstarDev.Plugins.XpubExtractor) を使って xpub 情報を提供する
4. 参加者側でウォレットデータを保存し、それをあなたに共有してもらいます。

現時点では、ユーザーは Store `Owners` として招待する必要があります。今後、マルチシグストアに招待する相手への信頼要件を下げるため、追加のロールと権限を実装する予定です。この実装状況は [GitHub で追跡](https://github.com/btcpayserver/btcpayserver/issues/6651) できます。

## Step 2: マルチシグウォレットを作成する

必要な公開鍵（例: 2-of-3 構成）をすべて収集したら、次の手順で進めます。

1. `Bitcoin Wallets` に移動します。
2. `Connect an existing wallet` > `Enter extended public key` を選択します。
3. `Show multisig examples` を選択し、収集した xpub を必要な形式で入力します。
4. `Continue` をクリックして、導出アドレスを検証しプレビューします。プレビューは外部ソフトウェアウォレットでも検証できますし、このドキュメントの最終ステップでテストしても構いません。

マルチシグの例は次のようになります。

```
2-of-xpub6BosLW1vGZLkCW7NrgUQdREa7i3a7XJXnAMQzA3aJCEuEt8hXNRu2yG6Vxq2YvCCu8n2eUpZhVz5Zw3eQro2d5Wq8UdD2qhM1YG4ZcnC3kYd-xpub6FHVXph13QMR77fUMLREpN2L7D1fCqcVtzsGhM28jUy5CWTpYHDuN6gvN5Gi5rxJjL45AgXLhSzE27AHZkDwKZgTYaUmYc9rBoF2tuAgf6M-xpub6CJ61yVNhtEtcpS7pU8Jjpd1NHgAG6xWv1NG4b47SvhhVVqfzFrHdcDUpm96B2ftov3qd5uoy6b7bEVcdxQwK6R7T5ndJP8vTWTdS6RBn7Jr
```

これは、列挙された 3 つの署名のうち 2 つがトランザクション承認に必要であることを意味します。同様に、3-of-5 などの構成も可能です。

次に、各種形式との互換性を高め、署名を容易にするためにウォレット設定を調整します。

1. `Wallet Settings` に移動します。
2. `Multisig on Server` オプションを有効にします。
3. 各鍵の `root fingerprint` と `derivation path` を入力します。
4. 互換性向上のために `Include non-witness UTXO in PSBTs` を有効にします。
5. 変更を `Save` します。

![](./img/multisig/multisig-configuration.png)

### （任意）受取設定のテスト

1. メインのマルチシグストアで、サイドバーの `Bitcoin > Receive` をクリックします
2. アドレスにラベルを付けます（例: "Test"）
3. 少額の Bitcoin を送金して設定を確認します
4. 必要に応じて、別のソフトウェアにマルチシグをインポートして受け取りが機能するか確認できます。

## 3. メール通知の設定

このステップにより、新しいトランザクションが作成されたとき、署名が必要なとき、最終的に正常にブロードキャストされたときに、参加者全員へメールが届くようになります。

マルチシグ活動の更新を参加者が受け取るには、[custom email-rules](https://docs.btcpayserver.org/Notifications/#email-rules) を設定する必要があります。

![](./img/multisig/multisig-email-rule-configure.png)

![](./img/multisig/multisig-email-rule-create.png)

1. `Store Settings > Emails > Email Rules` に移動します
2. `Configure` をクリックしてから `Create` をクリックします
3. `Trigger` ドロップダウンで `Pending Transaction Created` を選択し、マルチシグ参加者のメールアドレスをカンマ区切りで追加します: `email1@test.com, email2@test.com, email3@test.com`
4. 既定の Body と Text は必要に応じて変更できます。
5. 完了したら `Save` をクリックします

手順 3 から 5 を、次の 2 つのトリガーについても繰り返します: `Signature Collected` と `Transaction Broadcasted`

## Step 4: マルチシグウォレットからトランザクションを送信する

1. マルチシグウォレットに移動します
2. 送金先アドレスと金額を入力します
3. `Create Pending Transaction` をクリックします
4. Step 3 を実施していれば、参加者にはメールが届き、各自のウォレットでトランザクション署名を求められます。


## Step 5. マルチシグトランザクションに署名する:

トランザクションが作成されたので、参加者はハードウェアウォレットで署名（承認）する必要があります。

1. メインのマルチシグストアのウォレットトランザクション一覧で、参加者は保留中のトランザクションを確認できます。
2. `View` をクリックします
3. ハードウェアウォレットで署名する場合は、ハードウェアウォレットを接続し、`BTCPay Vault` が動作していることを確認します
4. `Sign` をクリックします
5. 画面およびデバイスの指示に従って `sign the transaction` を実行します

![](./img/multisig/multisig-view.png)

![](./img/multisig/multisig-choose-method.png)

![](./img/multisig/multisig-signed-1.png)

他の参加者も同じ手順に従います。必要な署名数が揃ったら、Broadcast をクリックしてトランザクションを送信します。

![](./img/multisig/multisig-broadcast.png)

![](./img/multisig/multisig-broadcast-2.png)

これで、BTCPay Server を使った最初のマルチシグトランザクション送信が完了です。
