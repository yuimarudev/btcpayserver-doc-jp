# Electrum Personal Server (EPS) 統合

[Electrum Personal Server (EPS)](https://github.com/chris-belcher/electrum-personal-server) は、[ElectrumX](./ElectrumX.md) のような公開 Electrum サーバーの個人向けバージョンです。

**EPS は BTCPay Server に統合できます**。任意の docker フラグメント [opt-add-electrum-ps.yml](https://github.com/btcpayserver/btcpayserver-docker/blob/master/docker-compose-generator/docker-fragments/opt-add-electrum-ps.yml) を使います。Electrum Wallet 利用時に、自分のトランザクションをプライベートに検証するため、自分のフルノード（BTCPay Server に同梱）を使いたい場合に EPS を利用してください。

公開 Electrum サーバー（例: ElectrumX）との最大の違いは、**EPS が自分のウォレットのみを監視するためのもの**である点です。機能させるには、Electrum で使うウォレットの "XPUB"（拡張公開鍵）を EPS に共有する必要があります。これ以外は、エンドユーザー視点では ElectrumX などと同様に動作します。**BTCPay への統合は簡単**なので、以下の手順に従ってください。

EPS は `txindex` を必要とせず、プルーニングノードでも動作します。

## Tor サポートについて

デフォルトでは、EPS は Tor 経由でアクセス可能です。サーバーに SSH で接続し、次のコマンドで Tor アドレスを取得できます。

```bash
cat /var/lib/docker/volumes/generated_tor_servicesdir/_data/btc-electrum-ps/hostname
```

BTCPay Server > Server Settings > Services から `Other TOR hidden services` の Tor リンクを確認することもできます。

Electrum Wallet を使うマシン側で Tor 経由接続する場合、このチュートリアルでは Tor Browser をローカル実行している前提のため、SOCKS5 ポート `9150` を使います。コマンドライン経由で Tor を動かしている場合、ローカル SOCKS5 ポートは `9050` です。

## BTCPay で Electrum Personal Server (EPS) を有効化する方法:

1. **Tor を使わない場合**、EPS は TCP ポート 50002 で Electrum Wallet からアクセスできます。少なくとも自分のネットワーク内で Electrum Wallet を動かす PC や Android デバイスから到達できるようこのポートを開放し、ポートフォワーディングを設定する必要があります。Tor を使うならこの手順は不要です。

2. EPS は単一ウォレット（単一ユーザー）向けなので、EPS の docker-fragment を有効にする前に、ウォレットの XPUB/YPUB/ZPUB を環境変数として指定する必要があります。Electrum Wallet で "Wallet" メニューから "Information" を開き、値をコピーしてください。次の手順でウォレット XPUB の ENV 変数を設定し、BTCPay ノードで Docker Additional Fragment を有効化します。

```bash
export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-add-electrum-ps"
export EPS_XPUB="XPUB_ADD_YOUR_XPUB_YPUB_OR_ZPUB_HERE"
. btcpay-setup.sh -i
```

3. Bitcoin フルノードと EPS サーバーの同期完了まで待ちます。
   Bitcoin Core の同期状態は、BTCPay Server のドメインを開くとトップページで確認できます。あるいはコマンドラインでも確認できます。
   `docker logs btcpayserver_bitcoind` - Bitcoin Core のブロックチェーン同期状態（およびノードの全情報、エラー含む）を表示します。
   `docker logs generated_electrum_ps_1` - EPS の同期状態を表示します。注意: EPS は Bitcoin フルノードの同期完了後まで同期を開始しません。完了まで表示されるエラーは無視して構いません。

Bitcoin と EPS の同期が両方完了したら、次の手順に進めます。（注意: 同期未完了の EPS サーバーには Electrum Wallet は接続できません）

## Electrum Wallet を EPS に接続する方法

Electrum Wallet からサーバーを利用する方法は3つあります。

1. 設定ファイルを編集する
2. コマンドラインで Electrum を起動する
3. ユーザーインターフェース経由で設定する（非推奨、プライバシー面で不利）

#### Option 1: Electrum Wallet の設定ファイルを直接編集して EPS サーバーに接続する（GUI を開く前・完全なプライバシーのため推奨）

設定ファイルを編集して **Electrum サーバーを設定**できます。

[Electrum Wallet folder](https://electrum.readthedocs.io/en/latest/faq.html#where-is-my-wallet-file-located) で `config` ファイルを開き、次のように編集します。

1. `"auto_connect": true,` を `"auto_connect": false,` に変更します。これにより、Electrum Wallet 起動時に他のサードパーティ Electrum サーバーへ自動接続してブロックヘッダーやトランザクション情報を取得する動作を防げます。

2. `"oneserver": false,` を `"oneserver": true,` に変更します。これにより、すべてのデータを1台のサーバーから取得するようになります。

3. `"server": "yourserver:50002:s",` を見つけるか追加し、自分の EPS サーバー IP アドレスに変更します。上の例なら `"server": "192.168.1.3:50002:s",` です。Wallet 起動時のデフォルト接続先として自分の IP を固定します。

これら3手順は、Electrum Wallet を自分のプライベートサーバー1台にのみ接続固定することで完全なプライバシーを実現するため、強く推奨されます（[Reference](https://github.com/chris-belcher/electrum-personal-server#how-to)）。

4. （**Tor を使う場合**）Tor Browser を実行しているなら、設定ファイルに `"proxy": "socks5:127.0.0.1:9150::",` を追加して SOCKS5 プロキシとして使用できます。

#### Option 3: コマンドラインで EPS サーバーに接続する

コマンドラインで `electrum --oneserver --server yourserver:50002:s` を実行して Electrum を起動できます。

Tor を使う場合は `-p socks5:localhost:9150` を追加します。

#### Option 4: Electrum Wallet GUI から EPS サーバーに接続する（オンライン時に一時的に他の公開 Electrum サーバーへ接続するため非推奨）

1. Electrum Wallet を開きます。画面下部の信号アイコン（緑または赤）をクリックすると、Wallet が接続可能な Electrum サーバー一覧の画面が表示されます。通常は `Select Server Automatically` がすでにチェックされています。

![ElectrumWalletServerList](https://user-images.githubusercontent.com/1388507/68437521-8a5eb580-01c1-11ea-9ece-0666353a6742.png)

2. ここで `Select Server Automatically` のチェックを外し、EPS サーバーの IP アドレス・ドメイン・ホスト名を入力できるようにします。以下の例ではローカルネットワークの `192.168.1.3` を入力しています（ポートは 50002 のまま）。“close” を押します。

![EnterElectrumServerIP](https://user-images.githubusercontent.com/1388507/68496320-4e276580-0252-11ea-8caf-facc8a246d70.png)

4. （**Tor を使う場合**）proxy に移動し、`Use Tor Proxy at port 9150` をクリックします。

5. ここまでが正しく完了し、ノードが健全で同期済みなら、Wallet 画面右下の信号アイコンが緑になります。これで成功です。

### 達成できたことの振り返り:

これで、あなた自身の**プライベート EPS サーバー**を運用できています。Electrum Wallet 関連データは、他のサードパーティサーバーを経由せず、EPS サーバーと Bitcoin ブロックチェーンの間で直接やり取りされます。少なくともブロックチェーンクエリ、トランザクション、支払い/受け取りアドレスの観点では、完全な Bitcoin 取引プライバシーを達成できます（あなたとブロックチェーン以外に行動を把握できません）。

### トラブルシューティング:

正しく設定しても、上記手順で赤い信号（どのサーバーにも未接続）が出る場合があります。追加のトラブルシュート情報は、このドキュメントに直接 PR で追加することをおすすめします。

- 赤い信号が表示されたら、Electrum Wallet を完全終了します。その後 Electrum Wallet folder を開いてください（場所が不明な場合は [see here](https://electrum.readthedocs.io/en/latest/faq.html#where-is-my-wallet-file-located)）。

Electrum Wallet folder 内（以下の例は Mac）で `certs` ディレクトリを見つけ、接続先サーバーの証明書（この例では `192.168.1.3`）をゴミ箱へ移動して削除します。

![Certs](https://user-images.githubusercontent.com/1388507/68497330-9a73a500-0254-11ea-9349-71bdb3bd9511.png)

Electrum Wallet を再起動し、**EPS server** に接続します。同期が完了していれば、信号アイコンは緑になり、利用可能な状態になります。
