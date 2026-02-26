<!-- legacy-anchor-aliases -->
<span id="security-implications"></span>
<!-- /legacy-anchor-aliases -->

# 新しいウォレットを作成する

- [ホットウォレット](#hot-wallet)
- [ウォッチ専用ウォレット](#watch-only-wallet)

### ホットウォレット <a name="hot-wallet"></a>

既存のウォレットがない場合は、BTCPay Server 内で新しく生成できます。既存ウォレットの有無にかかわらず、ストアに最短でウォレットを接続する方法は新しいウォレットの作成です。ストアをすぐに準備したい場合は、サーバーで少額決済をいくつか受け取った後で、いつでも別のウォレットに置き換えられます。

このタイプのウォレットは、[Payjoin](./Payjoin.md) や [Liquid](https://github.com/btcpayserver/btcpayserver/issues/1282) のような機能を使うためにも必要です。

ストアを作成した後、まずサイドバーまたはスライドアウトメニューに移動し、**Wallets** 見出しの下にある **Bitcoin** ボタンをクリック/タップすることでウォレットを紐づけられます。あるいは、Dashboard で **Set up a wallet** のオプションを見つけることもできます。

![Main Menu](./img/FirstStoreCreation.png)

ホットウォレットの場合は **I don't have a wallet** セクションを使い、**Create a new wallet** ボタンをクリックします。

![New Wallet](./img/hotwallet/CreateNewWallet.png)

次のページには 2 つの選択肢が表示され、この場合は **Hot wallet** ボタンを選びます。

![Create Wallet](./img/hotwallet/HotWallet.png)

ほとんどのユーザーにとって、**Address Type**（Segwit）を含むデフォルト設定で問題ありません。何をしているか確信がある場合を除き、変更しないことを推奨します。**Payjoin** 機能は任意で、詳細は[上記](#hot-wallet)のリンクで確認できます。

![Wallet Settings](./img/hotwallet/WalletSettings.png)


#### 詳細設定

- 任意の [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki#from-mnemonic-to-seed) パスフレーズ

  - ホットウォレットのニーモニックにパスフレーズを追加すると、セキュリティを一段強化できます。

- RPC へキーをインポート

  - これは BTCPay Server のより高度な用途向けです。キーを RPC にインポートすると、インポート済みウォレットで [bitcoind Wallet RPCs](https://developer.bitcoin.org/reference/rpc/index.html#wallet-rpcs) を活用できます。

![Advanced Settings](./img/hotwallet/AdvancedSettings.png)


#### リカバリーシード

ホットウォレット作成の最終ステップは、リカバリーシードを記録することです。リカバリーシードにアクセスできる人は、そこから秘密鍵を導出して現在および将来のすべての資金にアクセスし、盗むことができるため、非常に重要です。シードは紙などに書いて安全な場所に保管してください。写真撮影やデジタル保存は避けてください。リカバリーシードの保管をサーバーだけに依存せず、必ずバックアップを保持してください。

記録が完了したら、_I have written down my recovery phrase and stored it in a secure location_ のチェックボックスにチェックを入れ、**Done** ボタンをクリックします。

#### ウォレット作成の要件

[第三者ホスト](/Deployment/ThirdPartyHosting.md)を使用している場合、このオプションはサーバー管理者が明示的に有効化する必要があります。信頼できるか不明な環境で新しいウォレットを生成することは推奨されません。

デフォルトでは、ウォレット作成機能を使えるのはサーバー管理者のみです。これは、サーバー管理者が秘密鍵を容易に抽出できるためです。ただし、信頼できる他のメンバーにもストア作成と管理を任せたい場合は、非管理者向けにホットウォレット機能を有効化できます。設定は Server Settings > Policies > "Allow non-admins to create hot wallets for their stores" です。

![BTCPay Server settings](./img/hotwallet/ServerSettings.png)

:::warning
新しいウォレットが生成されると、BTCPay Server は 12 語のリカバリーシードを表示します。初回表示後、ホットウォレットオプションが有効な場合を除き、リカバリーシードはサーバーから消去されます。
:::

#### BTCPay ホットウォレットで資金を使う

ウォレットで資金を受け取った後、支出したい場合は BTCPay Server 内でトランザクションに自動署名できます。

1. BTCPay Server で > Wallets > Bitcoin > Send に移動
2. 送金先アドレスと金額を入力
3. 手数料率、希望承認時間、送金額から手数料を差し引くかどうかなど、トランザクション設定を調整
4. トランザクションに署名
5. トランザクションを確認
6. トランザクションをブロードキャスト

![BTCPay Server Send page](./img/hotwallet/WalletSend.png)
![BTCPay Server Transaction Review and Broadcast page](./img/hotwallet/BroadcastConfirm.png)

#### セキュリティ上の影響

公開サーバーに秘密鍵を保存することにはリスクがあります。これは [Lightning Network](./LightningNetwork.md) の運用・利用リスクと類似しています（ただしバックアップで資金復元は可能です）。
**この機能で生成されたシードは必ずバックアップし、失っても問題ない額を超える資金を、それらの秘密鍵でいつでも使える状態にしないでください**。

#### リスクを下げる

前述のとおり、ウォレット作成機能には、サーバーまたはアカウントが侵害された場合に資金が盗まれるリスクがあります。このリスクを軽減するため、次を推奨します。

- 二要素認証または U2F 認証を有効化する
- 定期的に資金をコールドストレージへ移す

:::danger
ホットウォレットを使用している場合、他人にサーバーの SSH キーやサーバーアカウント認証情報を渡さないでください。アカウントにアクセスできる人は、ホットウォレットの資金を使用できます。従業員・開発者などにアカウントアクセスを許可する必要がある場合は、代わりに[既存ウォレット](ConnectWallet.md#connect-an-existing-wallet)を使用してください。
:::

### ウォッチ専用ウォレット <a name="watch-only-wallet"></a>

ホットウォレットと同様に、ウォッチ専用ウォレットでもストアを即座にウォレットへ接続できます。対照的に、この方式では秘密鍵をサーバーに保存しません。その結果、このウォレットは受け取った資金に対して「watch-only」となります。

このタイプのウォレットで資金を使う方法はいくつかあります。たとえばシード語をハードウェアウォレットへインポートして [BTCPay Server Vault application](https://docs.btcpayserver.org/Vault/)、[PSBT](https://docs.btcpayserver.org/Wallet/#psbt) で署名する方法や、推奨度は低いですが毎回シード語を手動入力する方法があります。

![BTCPay Server Transaction Signing Options](./img/hotwallet/SignTransaction.png)

あるいは、BTCPay Server が生成したシード語をインポートした別の外部ウォレットで資金を使うこともできます。外部ウォレットが対応していれば、PSBT を使って資金を使うことも可能です。このオプションはウォレットの送金ページで利用できます。ウォッチ専用ウォレットを外部ウォレットと使う場合は、[ギャップリミットの問題](./FAQ/Wallet.md#missing-payments-in-my-software-or-hardware-wallet)に注意してください。
