# ハードウェアウォレット連携

使いやすさ・セキュリティ・プライバシーの最適なバランスのため、[BTCPay Server Wallet](Wallet.md) とハードウェアウォレットを組み合わせて使うことを推奨します。

BTCPay Server のハードウェアウォレット連携では、ハードウェアウォレットをインポートし、デバイス上で簡単に確認して受け取った資金を使用できます。秘密鍵はデバイス外へ出ることはなく、すべての資金は自分のフルノードに対して検証され、データ漏えいも防げます。

## はじめに

[![BTCPay Server Vault](https://img.youtube.com/vi/s4qbGxef43A/mqdefault.jpg)](https://www.youtube.com/watch?v=s4qbGxef43A)

1. [BTCPay Vault アプリをダウンロード](https://github.com/btcpayserver/BTCPayServer.Vault/releases)
2. PC（Windows、MacOS、Linux）に Vault をインストール
3. BTCPay Vault アプリを起動
4. ハードウェアウォレットを PC に接続し、スリープ解除状態であることを確認
5. 既存ストアがありますか？ある場合は手順 7 へ進んでください。
6. 既存ウォレットを接続し、`Connect a hardware wallet` をクリック
7. BTCPay Server がハードウェアウォレットの検索を開始します。この手順では BTCPay Server Vault を実行している必要があります。
8. BTCPay Vault アプリで承認をクリックします。Vault がデバイスを検索し、デバイス上で PIN の入力を求めます。
9. デバイスが見つかって承認されたら、アドレスタイプを選択して確認します。BTCPay Server にハードウェアウォレットの公開鍵情報が表示されます。
10. 公開鍵が正しいことを確認すると、BTCPay Server がデバイス上で検証するアドレスを表示します。正しければ確認してセットアップを完了します。

![BTCPay Server Vault configuration](./img/hww-setup/1-store-created.png)

![BTCPay Server Vault configuration](./img/hww-setup/2-connect-wallet.png)

![BTCPay Server Vault configuration](./img/hww-setup/3-choose-import-method.png)

![BTCPay Server Vault configuration](./img/hww-setup/4-vault-notif.png)

![BTCPay Server Vault configuration](./img/hww-setup/5-address-type.png)

![BTCPay Server Vault configuration](./img/hww-setup/6-pubkey-hww.png)

![BTCPay Server Vault configuration](./img/hww-setup/7-confirm-addresses.png)

![BTCPay Server Vault configuration](./img/hww-setup/8-wallet-setup-complete.png)

### 資金を送る

ウォレットに資金を受け取り、送金したい場合は、BTCPay Server 内からハードウェアウォレットでトランザクション署名できます。

1. PC で BTCPay Vault アプリを開く
2. ハードウェアウォレットを接続し、スリープ解除状態であることを確認
3. BTCPay Server で Bitcoin Wallet に移動し、`send` をクリック
4. 宛先アドレスと金額を入力
5. `Sign with a hardware wallet` を選択
6. ハードウェアウォレットでトランザクションを確認して承認
7. トランザクションをブロードキャスト

![Send Bitcoin via BTCPay Vault](./img/hww-setup/9-send-btc.png)

![Send Bitcoin via BTCPay Vault](./img/hww-setup/10-choose-signing-method.png)

![Send Bitcoin via BTCPay Vault](./img/hww-setup/11-sign-transaction.png)

## 詳細設定

追加のトランザクション設定は [Advanced Settings](Wallet.md#advanced-settings) ボタンから開けます。これらの設定に慣れていない場合は、既定値のままで利用できます。

Trezor ウォレットからの送信で問題がある場合は、[この詳細設定](FAQ/Wallet.md#why-is-sending-a-transaction-using-trezor-failing) を有効にする必要があることがあります。

![Send Bitcoin via BTCPay Vault](./img/hotwallet/BroadcastConfirm.png)

## 対応ハードウェアウォレット

対応デバイス一覧は [こちらのリンク](https://github.com/bitcoin-core/HWI#device-support) を参照してください。

:::warning
BTCPay Server のハードウェアウォレット連携は Bitcoin のみ対応です。サーバーで有効化した [Altcoin](/Development/Altcoins.md) ウォレットは利用できません。
:::
