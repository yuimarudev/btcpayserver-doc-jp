# reverse SSH トンネルでポートを転送する

## 利点

- ホスト側 LAN でポートフォワーディングが不要
- 接続が暗号化される
- ホストの IP を隠せる

## 要件

- Virtual Private Server (VPS)（例: Lunanode の最小プランで約 3.5$/月）
- VPS で root アクセス可能であること（1000 未満のポート転送は root のみ可能）
- ホストコンピューター（ポート転送元）への ssh アクセス

## セットアップ

### ホスト側（BTCPay Server インスタンス）

```bash
# switch to root user (if not logged in as root)
sudo su -

# check for an existing ssh public key
cat ~/.ssh/*.pub
```

公開鍵がなければ生成します（ENTER を押し続けます）。

```bash
ssh-keygen -t rsa -b 4096
```

これにより `~/.ssh` 内に SSH キーペア `id_rsa`（秘密鍵）と `id_rsa.pub`（公開鍵）が生成されます。

秘密鍵は ssh-agent に追加する必要があります。

```bash
# start the ssh-agent in the background
eval $(ssh-agent -s)

# add private key to ssh-agent
ssh-add ~/.ssh/id_rsa
```

公開鍵を VPS にコピーします（`VPS_IP_ADDRESS` を入力）。
VPS の root パスワード入力が求められます。

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub root@VPS_IP_ADDRESS
```

動作確認として VPS に SSH 接続します。成功すればパスワードはもう求められません。

```bash
ssh root@VPS_IP_ADDRESS
```

### VPS 側

先ほどの接続を再利用しても、root で再ログインしても構いません。

sshd 設定を編集します。

```bash
sudo nano /etc/ssh/sshd_config
```

次の項目が有効になっていることを確認してください（行頭に `#` がない状態）。
または、以下をファイル末尾に貼り付けても構いません。

```
RSAAuthentication yes     # not needed on latest OpenSSH versions
PubkeyAuthentication yes
GatewayPorts yes
AllowTcpForwarding yes
ClientAliveInterval 60
```

保存は CTRL+O, ENTER、終了は CTRL+X です。

:::warning
この時点で sshd 設定が誤っているとアクセス不能になる可能性があります。必ず再確認してください。
:::

sshd サービスを再起動します。

```bash
sudo systemctl restart sshd
```

### ホスト側（BTCPay Server インスタンス）に戻る

#### autossh のインストールと設定

依存パッケージ `autossh` をインストールします。

```bash
sudo apt-get install autossh
```

サービスファイルを作成します。

```bash
sudo nano /etc/systemd/system/autossh-tunnel.service
```

以下を貼り付け、`VPS_IP_ADDRESS` を入力します。
必要に応じてポートを追加・削除してください。

```ini
[Unit]
Description=AutoSSH tunnel service
After=network.target

[Service]
User=root
Group=root
Environment="AUTOSSH_GATETIME=0"
ExecStart=/usr/bin/autossh -C -M 0 -v -N -o "ServerAliveInterval=60" -R 9735:localhost:9735 -R 443:localhost:443 -R 80:localhost:80 root@VPS_IP_ADDRESS
StandardOutput=journal

[Install]
WantedBy=multi-user.target
```

サービスを有効化して起動します。

```bash
sudo systemctl enable autossh-tunnel
sudo systemctl start autossh-tunnel
```

これで reverse ssh-tunnel によるポート転送は完了です。
VPS の IP 経由でホストコンピューターのポート/サービスにアクセスできるはずです。

## 監視

ホストコンピューターでエラーがないか確認します。

```bash
sudo journalctl -f -n 20 -u autossh-tunnel
```

VPS 側でトンネルが有効か確認するには:

```bash
netstat -tulpn
```

## 参考資料

- Raspiblitz FAQ: [How to setup port-forwarding with a SSH tunnel?](https://github.com/rootzoll/raspiblitz/blob/master/FAQ.md#how-to-setup-port-forwarding-with-a-ssh-tunnel)
- RaspiBolt Docs: [Login with SSH keys](https://raspibolt.org/guide/raspberry-pi/security.html#login-with-ssh-keys)
