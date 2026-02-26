# Dynamic DNS サービス

## 背景

次の場合は **Dynamic DNS** が必要です。

- デフォルトドメインを提供しないホスティングプロバイダーで BTCPay Server を運用している
- 独自ドメイン（例: `mybusiness.com`）を購入したくない
- BTCPay Server にインターネット経由で HTTPS アクセスする必要がある（他のインターネット利用者が BTCPayServer にアクセスする）

この場合、**BTCPayServer Dynamic DNS service** の利用を推奨します。

次の場合は Dynamic DNS Service は **不要** です。

- 自宅で BTCPay Server をホストし、ローカルネットワーク経由でのみアクセスする（ローカル HTTP または Tor 利用で十分）
- BTCPay Server を自分だけが利用する（Tor Browser とインスタンスの Tor アドレスを使用）
- ホスティングプロバイダーがデフォルトでドメイン名を提供する（例: Lunanode は `.lndyn.com` のサブドメインを無料提供、Azure は `.azurewebsites.net` を提供）

**Dynamic DNS Providers** を使うと、サーバー用に `example.ddns.net` のような無料ドメインを持てます。
さらに Dynamic DNS Providers は、BTCPay Server インスタンスの外部 IP が変わったときに DNS レコードを自動更新するための簡単な API を提供します。

Dynamic DNS を設定した BTCPay Server は、外部 IP 変更を検知すると定期的に DNS レコードを確認し、更新します。

## 使い方

### ステップ 1: ドメインを作成する

まず Dynamic DNS プロバイダーでアカウントを作成します。代表的なプロバイダーは次のとおりです。

- [noip](https://www.noip.com/) (free)
- [duckdns](https://www.duckdns.org/) (free)
- [zoneedit](https://www.zoneedit.com/) (free)
- [dyndns](https://dyn.com/) (not free)
- [google](https://domains.google.com/) (not free)
- [easydns](https://www.easydns.com/) (not free)

アカウント作成後、各プロバイダーのウェブサイトから無料ドメイン名を作成できます。

### ステップ 2: BTCPay Server で Dynamic DNS を設定する

この操作にはインスタンス管理者権限が必要です。
Server Settings > Services > Dynamic DNS に移動します。

- Dynamic DNS を追加
- Dynamic DNS プロバイダーを選択
- ステップ 1 で作成したドメインを入力
- ステップ 1 で作成したログイン情報とパスワードを追加
- `enabled` ボックスにチェックして保存

### ステップ 3: HTTPS 証明書を提供するよう BTCPay の docker インストールを設定する

docker デプロイを使用している場合は、BTCPayServer インストール側も更新する必要があります。
SSH でインスタンスに接続し、次を実行します。

```bash
BTCPAY_ADDITIONAL_HOSTS="example.ddns.net"
. btcpay-setup.sh -i
```

`BTCPAY_ADDITIONAL_HOSTS` に他のホストがある場合は、`,` で区切って追加してください。
