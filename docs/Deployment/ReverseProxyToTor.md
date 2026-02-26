# Tor へのリバースプロキシ

## 利点

- ホストの LAN 側でポートフォワーディングが不要
- 接続が暗号化される
- ホストの IP を隠せる

## 要件

- Virtual Private Server (VPS) - 例: Lunanode の最小プランで約 3.5$/month
- VPS の root 権限 - Web サーバーの設定とパッケージのインストールが必要
- ドメインまたはサブドメイン - プロキシ Web サーバーに設定します

BTCPay Server の Tor `.onion` アドレスは `Server settings > Services` ページで取得します。
"HTTP-based TOR hidden services" セクションの情報を参照してください。

注: この構成には [Docker version](#do-all-this-in-a-docker-container) もあります。

## VPS セットアップ

`nginx` のリバースプロキシと、BTCPay Server へリクエストを転送する `socat` サービスを作成します。

root でログインし、必要な依存関係をインストールします（以下は Debian 系システムを想定）。

```bash
# switch to root user (if not logged in as root)
sudo su -

# install dependencies
apt update
apt install -y certbot nginx socat tor
```

### Socat のセットアップ

サービスファイル `/etc/systemd/system/http-to-socks-proxy@.service` を作成します。

```ini
[Unit]
Description=HTTP-to-SOCKS proxy
After=network.target

[Service]
EnvironmentFile=/etc/http-to-socks-proxy/%i.conf
ExecStart=/usr/bin/socat tcp4-LISTEN:${LOCAL_PORT},reuseaddr,fork,keepalive,bind=127.0.0.1 SOCKS4A:${PROXY_HOST}:${REMOTE_HOST}:${REMOTE_PORT},socksport=${PROXY_PORT}

[Install]
WantedBy=multi-user.target
```

`/etc/http-to-socks-proxy/btcpayserver.conf` にサービス設定を作成します。

```bash
# create the directory
mkdir -p /etc/http-to-socks-proxy/

# create the file with the content below
nano /etc/http-to-socks-proxy/btcpayserver.conf
```

`REMOTE_HOST` を置き換え、必要に応じてポートを調整します。

```conf
PROXY_HOST=127.0.0.1
PROXY_PORT=9050
LOCAL_PORT=9081
REMOTE_HOST=heregoesthebtcpayserverhiddenserviceaddress.onion
REMOTE_PORT=80
```

`/etc/systemd/system/multi-user.target.wants` にシンボリックリンクを作成してサービスを有効化し、起動します。

```bash
# enable
ln -s /etc/systemd/system/http-to-socks-proxy\@.service /etc/systemd/system/multi-user.target.wants/http-to-socks-proxy\@btcpayserver.service

# start
systemctl start http-to-socks-proxy@btcpayserver

# check service status
systemctl status http-to-socks-proxy@btcpayserver

# check if tunnel is active
netstat -tulpn | grep socat
# should give something like this:
# tcp        0      0 127.0.0.1:9081          0.0.0.0:*               LISTEN      951/socat
```

### Web サーバーのセットアップ

#### ドメインを VPS に向ける

ドメイン/サブドメインの DNS サーバーで A レコードを作成し、VPS の IP アドレスを指定します。

#### SSL と Let's Encrypt の準備

```bash
# generate 4096 bit DH params to strengthen the security, may take a while
openssl dhparam -out /etc/ssl/certs/dhparam.pem 4096

# create directory for Let's Encrypt files
mkdir -p /var/lib/letsencrypt/.well-known
chgrp www-data /var/lib/letsencrypt
chmod g+s /var/lib/letsencrypt
```

#### nginx 設定: http

正しいプロトコル設定を転送する変数マッピングを作成し、クライアントから Upgrade ヘッダーが送られているか確認します。例: `/etc/nginx/conf.d/map.conf`

```nginx
map $http_x_forwarded_proto $proxy_x_forwarded_proto {
  default $http_x_forwarded_proto;
  ''      $scheme;
}

map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}
```

ドメイン用の設定ファイルを作成します。例: `/etc/nginx/sites-available/btcpayserver.conf`

```nginx
server {
  listen 80;
  server_name btcpayserver.mydomain.com;

  # Let's Encrypt verification requests
  location ^~ /.well-known/acme-challenge/ {
    allow all;
    root /var/lib/letsencrypt/;
    default_type "text/plain";
    try_files $uri =404;
  }

  # Redirect everything else to https
  location / {
    return 301 https://$server_name$request_uri;
  }
}
```

SSL 証明書を取得した後、同じ設定ファイル内で https サーバー部分を設定します。

シンボリックリンクを作成して Web サーバー設定を有効化し、nginx を再起動します。

```bash
ln -s /etc/nginx/sites-available/btcpayserver.conf /etc/nginx/sites-enabled/btcpayserver.conf

systemctl restart nginx
```

#### Let's Encrypt で SSL 証明書を取得

メールアドレスとドメインを調整して、次のコマンドを実行します。

```bash
certbot certonly --agree-tos --email admin@mydomain.com --webroot -w /var/lib/letsencrypt/ -d btcpayserver.mydomain.com
```

#### nginx 設定: https

有効な SSL 証明書を取得できたので、`/etc/nginx/sites-available/btcpayserver.conf` の末尾に https サーバー部分を追加します。

```nginx
server {
  listen 443 ssl http2;
  server_name btcpayserver.mydomain.com;

  # SSL settings
  ssl_stapling on;
  ssl_stapling_verify on;

  ssl_session_timeout 1d;
  ssl_session_cache shared:SSL:10m;
  ssl_session_tickets off;

  # Update this with the path of your certificate files
  ssl_certificate /etc/letsencrypt/live/btcpayserver.mydomain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/btcpayserver.mydomain.com/privkey.pem;

  ssl_dhparam /etc/ssl/certs/dhparam.pem;
  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
  ssl_prefer_server_ciphers off;

  resolver 8.8.8.8 8.8.4.4 valid=300s;
  resolver_timeout 30s;

  add_header Strict-Transport-Security "max-age=63072000" always;
  add_header Content-Security-Policy "frame-ancestors 'self';";
  add_header X-Content-Type-Options nosniff;

  # Proxy requests to the socat service
  location / {
    proxy_pass http://127.0.0.1:9081/;
    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header X-Forwarded-Proto $proxy_x_forwarded_proto;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
  }
}
```

もう一度 nginx を再起動します。

```bash
systemctl restart nginx
```

`btcpayserver.mydomain.com` にアクセスして、BTCPay Server インスタンスが表示されれば成功です。

:::tip
`503 Service Temporarily Unavailable` などの nginx エラーが表示される一方で BTCPay Server 自体には到達できる場合、新しいドメインを BTCPay Server に認識させる必要があります。環境変数（Docker ベースの構成）で設定できます。SSH で BTCPay Server にログインして次を実行してください。

```bash
sudo su -
cd $BTCPAY_BASE_DIRECTORY/btcpayserver-docker/
export BTCPAY_ADDITIONAL_HOSTS="btcpayserver.mydomain.com"
. ./btcpay-setup.sh -i
```

:::

## これをすべて Docker コンテナで実行する

準備済みの [Docker image](https://hub.docker.com/r/cloudgenius/socator)（[Code](https://github.com/beacloudgenius/socator)）

### SocaTor = SOCAT + TOR

[Docker-Socator](https://github.com/Arno0x/Docker-Socator) ベース

このイメージは、指定した TCP ポート（この例では 5000）で待ち受けるために socat を使い、環境変数で指定した Tor hidden service へ受信トラフィックを転送します。
通常の Web と Tor ネットワーク上の hidden service の間を中継します。
`ALLOWED_RANGE` 環境変数に CIDR 記法で指定すると、このサービスへ接続できる IP アドレスを制限することも可能です。

注意:

このコンテナには nginx コンポーネントは含まれていません。Kubernetes 側で提供されるためです。

### 使い方

クラウドサービスプロバイダーの制約から解放され、Bitcoin フルノードを安全に保護し、BTCPay Server と接続し、すべてを TOR の背後で運用できます。
インターネット上で稼働する socat+tor を使い、BTCPay Server の決済ゲートウェイと API を選択的にクリーンネットへ公開できます。

---

#### ビルド

```sh
docker build -t cloudgenius/socator .
```

#### プッシュ

```sh
docker push cloudgenius/socator
```

#### IP アドレス制限付きでバックグラウンド（_daemon mode_）起動

```sh
docker run -d \
        -p 5000:5000 \
        -e "ALLOWED_RANGE=192.168.1.0/24" \
        -e "TOR_SITE=zqktlwiuavvvqqt4ybvgvi7tyo4hjl5xgfuvpdf6otjiycgwqbym2qad.onion" \
        -e "TOR_SITE_PORT=80" \
        --name socator \
        cloudgenius/socator
```

#### フォアグラウンド起動

```sh
docker run --rm -ti \
        -p 5000:5000 \
        -e "TOR_SITE=zqktlwiuavvvqqt4ybvgvi7tyo4hjl5xgfuvpdf6otjiycgwqbym2qad.onion" \
        -e "TOR_SITE_PORT=80" \
        --name socator \
        cloudgenius/socator
```

`http://localhost:5000` にアクセスすると、上記コマンドで指定した Tor hidden service が表示されるはずです。

## これらのマニフェストを使って Kubernetes クラスターでこの Docker コンテナを利用する

これらのマニフェストは、典型的な Kubernetes クラスターを想定しています。内部サービス（例: ポート 5000 で内部稼働する socator）を [Nginx Ingress](https://github.com/kubernetes/ingress-nginx) 経由でクリーンネット/パブリックインターネットへ公開し、[cert-manager](https://github.com/jetstack/cert-manager) により Let's Encrypt の TLS/SSL 証明書を自動発行します。

Deployment manifest

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: socator
spec:
  replicas: 1
  selector:
    matchLabels:
      role: socator
  template:
    metadata:
      labels:
        role: socator
    spec:
      containers:
        - image: cloudgenius/socator # code https://github.com/beacloudgenius/socator
          imagePullPolicy: IfNotPresent
          name: socator
          env:
            - name: TOR_SITE
              value: 'zqktlwiuavvvqqt4ybvgvi7tyo4hjl5xgfuvpdf6otjiycgwqbym2qad.onion' # BTCPay Server Tor address => docker exec tor cat /var/lib/tor/app-btcpay-server/hostname
            - name: TOR_SITE_PORT
              value: '80'
```

Service manifest

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: socator
spec:
  ports:
    - name: http
      port: 5000
  selector:
    role: socator
```

Ingress manifest

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: socator
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  rules:
    - host: btcpayserver.mydomain.com
      http:
        paths:
          - backend:
              service:
                name: socator
                port:
                  number: 5000
            path: /
            pathType: Prefix
  tls:
    - hosts:
        - btcpayserver.mydomain.com
      secretName: socator-tls
```

## リソース

- [nginx reverse proxy to .onion site in Tor network](https://itgala.xyz/nginx-reverse-proxy-to-onion-site-in-tor-network/)
- [Tor-to-IP tunnel service](https://github.com/openoms/bitcoin-tutorials/blob/master/tor2ip_tunnel.md)
- [How to make a nginx reverse proxy direct to tor hidden service](https://stackoverflow.com/questions/55487324/how-to-make-a-nginx-reverse-proxy-direct-to-tor-hidden-service)
- [Secure Nginx with Let's Encrypt on Debian 10 Linux](https://linuxize.com/post/secure-nginx-with-let-s-encrypt-on-debian-10/)
- [Nginx WebSocket proxying](http://nginx.org/en/docs/http/websocket.html)
