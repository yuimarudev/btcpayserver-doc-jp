<!-- legacy-anchor-aliases -->
<span id="configuring-the-gap-limit-in-electrum"></span>
<!-- /legacy-anchor-aliases -->

# ElectrumウォレットをBTCPay Serverに接続する

このドキュメントでは、**デスクトップ版の[Electrum Wallet](https://electrum.org/)をBTCPay Serverに接続する方法**を説明します。

**注意:** Electrumウォレットは、サードパーティが管理するElectrumサーバーに依存しています。公開アドレス、残高、取引金額などの情報が _漏洩する可能性_ があります。

このような漏洩から身を守るには、[ElectrumX Server](./ElectrumX.md) または [Electrum Personal Server - EPS](https://github.com/chris-belcher/electrum-personal-server) をセットアップしてください。

EPS と ElectrumX の違いは[こちら](https://www.reddit.com/r/Electrum/comments/7xb0lz/whats_the_difference_between_electrumx_server_and/)で確認できます。

1. BTCPay Serverでストアを作成する
2. [Electrum Wallet](https://electrum.org/#download)をダウンロードしてインストールする

## Electrumウォレットのセットアップ

インストール後、デスクトップ上のアイコンをクリックして **Electrum Wallet** を開きます。

### クイックセットアップ

BTCPayでElectrumウォレットを設定する最も簡単な方法は、ウォレットのバックアップファイルをBTCPay Serverにインポートすることです。

1. 新しいElectrumウォレットを作成する
2. Electrumで `File > Save Backup > Save in folder` を選ぶ
3. BTCPay Serverで `Store > Settings > Setup > Import Wallet File > Choose File > Continue` を選ぶ
4. Electrumの `Receive` タブを開く
5. ElectrumとBTCPay Serverのアドレスを比較し、一致していることを確認する
6. BTCPayでアドレスの一致を確認する

## ステップバイステップ

以下の手順では、Electrumでまったく新しい Bech32(SegWit) ウォレットを作成します。すでにウォレットをお持ちの場合は、Extended Public Key のコピー手順まで進んでください。

まず、ウォレット名を入力します。例: `BTCPay Server Wallet`。その後 `Next` をクリックします。

![ElectrumWallet](./img/ElectrumWallet1.png)

`Standard wallet` を選び、`Next` ボタンをクリックして進みます。

![ElectrumWallet](./img/ElectrumWallet2.png)

今回は新規ウォレットを作成するため、`Create a new seed` を選択して `Next` をクリックします。

![ElectrumWallet](./img/ElectrumWallet3.png)

選択メニューで `SegWit` を選び、`Next` をクリックします。

![ElectrumWallet](./img/ElectrumWallet4.png)

**重要:** マーチャントの場合、SegWit (Bech32) ではなく SegWit wrapped (P2SH) 形式の利用を推奨します。[このガイド](https://www.youtube.com/watch?v=-1DBJWwA2Cw)では、顧客が使用するレガシーウォレットとの互換性の観点から、マーチャント向けのP2SHウォレット作成方法を説明しています。

**重要 2:** 画面に表示された順番どおりにリカバリーワードを書き留めてください。紙に書き、必ず安全な場所に保管してください。時間をかけて各単語を3回確認してください。シードをデジタル形式（写真、テキストファイルなど）で保存しないでください。シードにアクセスできる人はあなたの資金にもアクセスできます。同じ順番で再入力し、バックアップが正しくできていることを確認してください。シードの検証が完了したら次の手順に進みます。

Electrumでウォレット作成を完了するために、シード語を入力します。BTCPay Serverにインポートするにはウォレットが暗号化されていない必要があります。BTCPayでのセットアップ完了後、必要に応じてElectrum側で後からパスワード暗号化を設定できます。

BTCPay Serverへのインポート手順は以下の動画でも確認できます。

[![BTCPay Server - How to import wallet file](https://img.youtube.com/vi/kf3BHaQWSAc/mqdefault.jpg)](https://www.youtube.com/watch?v=kf3BHaQWSAc)

### 代替セットアップ

ウォレットファイルをインポートする代わりに、公開鍵をBTCPay Serverへ転送する方法もあります。ウォレットが暗号化されており、復号したくない場合に便利です。

1. 新しいElectrumウォレットを作成する
2. Electrumで `Wallet > Wallet Information` を開き、**Master Public Key** をコピーする
3. BTCPay Serverで `Store > Settings > Setup > Connect an existing wallet > Enter extended public key` を選ぶ
4. Electrumの `Receive` タブを開く
5. ElectrumとBTCPay Serverのアドレスを比較し、一致していることを確認する
6. BTCPayでアドレスの一致を確認する

ウォレットの読み込み後（数秒かかる場合があります）、上部メニューの `Wallet` をクリックし、次に `Information` をクリックします。

![ElectrumWallet](./img/ElectrumWallet9.png)

`Master Public Key` を選択して**コピー**します。これはBTCPayがアドレスを導出するための**公開**鍵です。

![ElectrumWallet](./img/ElectrumWallet10.png)

BTCPay Serverに戻ります。左側メニューの `Bitcoin` か、新しいダッシュボード上の `Set up a wallet` をクリックします。

![ElectrumWallet](./img/electrum/btcpayWalletImport1.jpg)

`Connect an existing wallet` をクリックします。

![ElectrumWallet](./img/electrum/btcpayWalletImport2.jpg)

次に `Enter extended public key` をクリックして鍵をインポートします。

![ElectrumWallet](./img/electrum/btcpayWalletImport3.jpg)

`Master Public Key` を derivation scheme フィールドへそのまま貼り付け、他は追加しないでください。`Enabled` チェックボックスがオンになっていることを確認して `Continue` をクリックします。

![ElectrumWallet](./img/createwallet/SetupWalletXpub.png)

**Electrum Wallet** に戻り、受取アドレスが表示される `Receive tab` を開きます。

**Electrum Walletで表示されるアドレスとBTCPay Serverに表示されるアドレスを比較してください**。一致していれば `continue` します。一致しない場合は、`Master Public Key` を正しく貼り付けているか再確認してください。

![ElectrumWallet](./img/ElectrumWallet11.png)

### ElectrumでGap Limitを設定する

上部メニューで `View` をクリックし、次に `Show Console` をクリックします。

![ElectrumWallet](./img/ElectrumWallet11a.png)

Electrumのコンソールで次のコマンドを入力し、キーボードの `enter` を押します。

```
 wallet.change_gap_limit(100)
```

Electrum 4より古いバージョンを使用している場合は、次のコマンドも入力して `enter` を押してください。

```
wallet.storage.write()
```

![ElectrumWallet](./img/ElectrumWallet12.png)

Electrumを再起動し、コンソールで次を入力して新しい gap limit が正しく設定されていることを確認します。

```
wallet.gap_limit
```

gap limit をいくつに設定すべきかに絶対的な正解はありません。多くのマーチャントは 100-200 を設定します。取引量が多い大規模マーチャントであれば、さらに高い値を試すこともできます。

[Gap Limitの詳細はFAQ](./FAQ/Wallet.md#missing-payments-in-my-software-or-hardware-wallet)を確認してください。

**これでElectrumとBTCPay Serverの接続は完了です**。BTCPayで受け取った支払いはElectrumに表示され、そこから支出できます。
