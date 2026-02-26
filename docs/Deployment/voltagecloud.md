# Voltage Cloud BTCPay Server Web デプロイ

このページでは、`Voltage Cloud` 上に BTCPay Server をデプロイする方法を説明します。
Voltage cloud は Bitcoin と Lightning のインフラを構築しており、ミッションは Bitcoin スタンダードに向けた簡単でスケーラブルなソリューションを提供することです。
BTCPay Server 向けに使いやすいオンボードフローを提供しており、Lightning Network の利用も簡単です。
モジュール式なので、Bitcoin ノードを選び、Lightning を使うかどうかを決め、その上に BTCPay Server を構築できます。
また、Lightning 稼働後のインバウンド流動性のソリューションも提供しています。

以下の動画では、[BTC Sessions](https://twitter.com/BTCsessions) が **Voltage Cloud 上で BTCPay server を設定する**全手順を解説しています。

[![BTCPay Server - Voltage Web-Deployment](https://img.youtube.com/vi/KqsM-n-e4aY/mqdefault.jpg)](https://www.youtube.com/watch?v=KqsM-n-e4aY)

## 1. サインアップしてアカウントにチャージする

まずサインアップしてアカウントにクレジットを追加します。
アカウント作成は [Voltage cloud](https://account.voltage.cloud/register) のサイトから行います。

![Voltage Cloud signup](../img/voltage/voltagereg.jpg)

登録後、アカウントにチャージするには `Billing` に移動し、`Node credit` を追加します。
チャージは Bitcoin、Lightning、またはクレジットカードで支払いできます。
支払い完了後、Voltage-Cloud のメインダッシュボードに戻ります。

## 2. 自分に合うノードを選ぶ

ダッシュボードで次の操作を選択します。
このセクションでは `Nodes` タブに入り、`Flow` はいったん使いません。
`Nodes` をクリックすると **Create Node** が表示されます。

Lightning 有効の BTCPay server を稼働させるには、2つのノードを作成する必要があります。
最初に Lightning Node を作成するため、LND ボタンをクリックします。
その後、3 種類のノードオプションから選択します。

- Lite node
- Standard Node
- Pro node

それぞれ長所と短所があります。Pro node はプランのカスタマイズのために Voltage の営業チームへの連絡が必要です。
ノードの時間単価と月額の概算も表示されます。
ただしこのガイドの範囲と BTCPay Server の要件であれば、`Lite node` で十分です。

![Voltage Cloud Password](../img/voltage/deployln.jpg)

次にノード名を設定し、強力なパスワードで保護します。
シードや Static Channel backup から既存ノードを復元するオプションもあります。

![Voltage Cloud Password](../img/voltage/voltagelnname.jpg)

:::warning
このパスワードは安全にバックアップしてください。Voltage はこれを**復元できません**。
:::

## 3. ノードダッシュボード

上記手順を完了すると、Voltage ノードのダッシュボードに入ります。
ここではノードの状態、実行中の LND バージョン、API エンドポイント、その他の情報タイルを確認できます。
ここから **BTCPay server node** の作成を進めます。

ダッシュボード右上では `Nodes` と `Flow` を切り替えられます。
BTCPay server node を作成したいので `Nodes` をクリックし、新しいノードを作成します。

![Voltage Cloud Node](../img/voltage/whatnode.jpg)

## 4. BTCPay server node

ここから BTCPay Server node を作成します。
LND ノード作成時と同様に、時間単価と月額概算が表示されます。

`Create` ボタンをクリックし、ストア情報を設定します。
`Store name` で BTCpay server のメインストア名を設定します。
先ほど作成した Voltage ノード（LND ノード）に接続する選択を行います。
最後に LND ノード作成時に設定したパスワードを入力して確定します。

![Voltage Cloud BTCPay server Node](../img/voltage/deploybtcpay.jpg)

## 5. 初回デプロイを完了する

ここまでの手順と上記動画に沿って進めると、Voltage に BTCPay server 情報が表示されます。

- BTCPay Server へアクセスする URL
- BTCPay Server ストアのユーザー名
- デフォルトパスワード
- 接続された Node 名
- ノード作成日
- 有効期限
- 購入ステータス

BTCPay Server ダッシュボードに簡単にアクセスするボタンと、インスタンス削除ボタンも利用可能になります。
BTCPay server ダッシュボードに進むボタンをクリックすれば、BTCPay の利用準備は完了です。

## 6. BTCPay Server ダッシュボードへようこそ

新しい BTCPay Server にアクセスできました。
ダッシュボードにはストア作成済みで Lightning Network が設定済みであることが表示されます。
Bitcoin ウォレットはまだ未設定です。[こちらのウォレット設定ガイド](../WalletSetup.md) を参照してください。

:::tip
ノード、デプロイ、アップデートに関する質問がある場合は、[Voltage Cloud](https://voltage.cloud) のサポートに問い合わせてください。
:::
