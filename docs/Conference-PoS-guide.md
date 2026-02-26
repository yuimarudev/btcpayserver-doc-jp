# カンファレンス / イベント / ローカルコミュニティ向け BTCPay Server セットアップ

ここでは、[Bitcoin Atlantis](https://blog.btcpayserver.org/case-study-bitcoin-atlantis/)、[Bitcoin Hong Kong](https://bitcoinmagazine.com/business/case-study-enabling-bitcoin-as-a-medium-of-exchange-at-the-bitcoin-asia-conference-in-hong-kong)、[Bitcoin Nashville](https://x.com/BtcpayServer/status/1856410949797683379) などのカンファレンスで実際に使ったセットアップを紹介します。参加者と出店者の双方にとって優れたユーザー体験を実現するための構成です。

Point of Sale (PoS) 端末には、レシートプリンター付きの [Bitcoinize](https://bitcoinize.com/) 端末を使います（ただし、他の Android ベース PoS 端末でも構いません）。

Lightning Network 部分は Blink プラグイン経由で [Blink.sv](https://blink.sv) を使います（Alby、Breez、内蔵 LN ノード、または自前 LN ノード接続など、他サービスに置き換え可能です）。

さらに、インターネット接続がない参加者や Bitcoin 知識がない参加者でもよりスムーズに使えるよう、Bolt Card の発行とトップアップを可能にする方法も説明します。

セットアップ全体と手順を俯瞰するには、BTCPrague 2024 のワークショップをご覧ください。
[![BTCPrague 2024 Workshop](https://img.youtube.com/vi/Hpd-IytvI4Y/1.jpg)](https://youtu.be/Hpd-IytvI4Y)

---

**目次**

[[toc]]

##  初期セットアップ
- 公開 IP 付き VPS に BTCPay Server v2.0.5 以降をセットアップ
- サブドメインを設定、または VPS ホスティング（例: Lunanode）が提供するものを使用
- 管理者アカウントを登録し、最初のテストストアを作成
- [FastSync](https://docs.btcpayserver.org/Docker/fastsync/) を実施（任意）


**サーバー起動後、次のプラグインをインストールします:**
- Blink plugin
- Bolt Cards plugin

その後、BTCPay Server を再起動します（UI から、または SSH で `docker restart generated_btcpayserver_1`）

## 出店者ストアのセットアップと Bitcoinize 端末の初期化

### Bitcoinize 端末の準備
- Bitcoinize 端末のバッテリー残量が 90% 以上であることを確認（不足時は充電）
- 10台以上を一度にセットアップする場合、延長コードや高出力 USB ハブが重要
- 端末プリンターに用紙ロールを装填

## 出店者ストアのセットアップ

管理者アカウントで BTCPay Server インスタンスにログインします。

***各出店者ごとに以下を繰り返します:***

### 1. ストア作成
- 左上ドロップダウン -> "**Create store**" ボタン
  - **Name**: 例: Nakamoto's Pineapple Pizza
  - **Default Currency**: 場所に応じて USD、HKD など
  - **Preferred Price Source**: USD/EUR なら Kraken 推奨、HKD なら Coingecko。よりマイナーな通貨は Coingecko を試すか、ドロップダウンでローカル取引所があるか確認
- "**Create store**" ボタンをクリック
### 2. Lightning Network ウォレット設定
出店者の Blink アカウントへ接続する Lightning ウォレットを設定します。[Blink docs の手順](https://dev.blink.sv/examples/btcpayserver-plugin#how-to-connect)に従ってください。
（代替として、出店者が法定通貨で受け取りたい場合は、カンファレンス側の Blink アカウントを設定し、後で fiat ramps 経由で分配する運用も可能です）

### 3. スプレッド設定とサウンド有効化
- "**Settings**" -> "**Rates**" をクリック
- **Add Exchange Rate Spread**: `1` を入力して余裕を持たせます（Blink は StableSats USD のヘッジで 0.2% を課金）
- "**Save**" ボタンをクリック
- "**Settings**" -> "**Checkout Appearance**" をクリック
- "**Select a preset**" ドロップダウンで "**In-store**" を選択
- "**Save**" ボタンをクリック

### 4. Point of Sale (PoS) 設定
- 左サイドバー「Plugins」配下の "**Point of Sale**" をクリック
- "**App Name**": ストアと同じ出店者名を入力
- "**Create**" ボタンをクリック
- PoS 設定画面で、"**App Name**" と "**Display Title**" が入力済みであることを確認
- "**Choose Point of Sale Style**": "**Keypad**" を選択
- "**Currency**": ストアと同じ通貨を選択
- "**Save**" ボタンをクリック
- 右上の "**View**" ボタンをクリックし、キーパッド表示を確認

### 5. Bitcoinize 端末への PoS リンク登録とラベル貼付

- ブラウザで PoS 設定に戻り、"**QR-code icon**" をクリック
- Bitcoinize（または他端末）で "**Camera**" アプリを開く
- 右にスクロールして "**more**" カテゴリ -> "**QR-Code**" を選択
- ブラウザに表示された QR コード（PoS 設定ページ）をスキャン
- スキャン後、Chrome ブラウザで URL を開く
- キーパッドと正しい出店者名が表示されることを確認
- 右上の 3 点 "**...**" をタップし、"**Add to home**" を選択
- 端末ホームのメイン画面にアイコンを配置してアクセスしやすくする
- 出店者名ステッカーで端末と箱にラベルを貼る

***PoS 支払いテストと権限付与:***
- ホーム画面から PoS ページを起動
- 端末の音量を上げ、支払い時に音声フィードバックが出るようにする（Bolt Card 利用時は特に重要）
- 0.01 USD（または同等額）を入力し、"**Charge**" ボタンをタップ
- 初回のみブラウザが NFC 権限を要求するため、許可ボタンをタップ
- Bolt Card または Lightning ウォレットで請求書を支払う
- 支払い後に音が鳴ることを確認
- "**View receipt**" ボタンをタップし、ドロップダウンで **POSPrinter** を選択、"**Print**" ボタンで印刷テスト

### 6. 出店者へ支払い履歴アクセス権を付与（任意だが推奨）

必要に応じて、各ストア/出店者ごとに PoS 端末でログインできるようにすると、支払い履歴を確認できます。直近支払いの確認や二重支払い防止に役立ちます。次の権限を持つ "Merchant" ロールを追加してください:
- btcpay.store.canmodifyinvoices
- btcpay.store.canviewstoresettings
- btcpay.store.canviewpaymentrequestes
- btcpay.store.canarchivepullpayments
- btcpay.store.cancreatenonapprovedpullpayments

その後、各ストア用ユーザーを作成して正しいストアに割り当てます。

### 7. 端末で出店者ログインをしない場合のレート制限引き上げ（任意）

デフォルトでは、`create invoice` エンドポイントは public リクエストに対して IP ごとに 1 分あたり 4 リクエストに制限されています。複数の POS 端末で同一 IP を共有していると、負荷テスト時に同時に 4 件を超える請求書を作成できないことがあります。

[2024年11月の PR](https://github.com/btcpayserver/btcpayserver/pull/6415) で、ログイン済みユーザーにはこの制限を適用しないようになりました。したがって **Step 6** を実施済みならこの問題は起きません。各端末でログインさせない運用にしたい場合は、次のように制限を引き上げられます。

1. BTCPay Server の **Manage Plugins** で Kukks 作成の **Dynamic Rate Limit** プラグインをインストール。
2. `/plugins/dynamicrateslimiter`（**Server Settings -> Dynamic Reports -> Rate Limits**）へ移動。
3. **Add Rate Limit** をクリックし、次の値を入力: `zone=publicinvoices rate=9999r/m burst=500 nodelay`

![Rate Limiting configuration setup for zone=publicinvoices](./img/conference-pos-guide/rate-limiting.png)

この変更により、任意の IP から 1 分あたり最大 9999 件まで請求書作成が可能になります。そのため、推奨アプローチは **Step 6**（端末でのユーザーログイン）です。

## Bolt Cards プロバイダのセットアップ

Bolt Cards プロバイダ専用の別ストアを作成します。ストア一覧で見つけやすいよう、ストア名の先頭に "z" を付けると便利です（例: "z - Bolt Cards Provider"）。

この特別ストアでは Blink アカウントを接続しますが、出店者向け Blink アカウントとは次の点が異なります:
- API key に **write permission** も必要（ないと Bolt Cards が資金を引き出せません）
- **Bitcoin wallet** を接続すること（*StableSats USD* ではない）

### 1. ストア作成
- "**z - Bolt Cards Provider**" という名前でストアを作成（手順は上の出店者ストアと同じ）

### 2. Lightning Network ウォレット設定

[Blink docs の手順](https://dev.blink.sv/examples/btcpayserver-plugin#how-to-connect)に沿って、Blink アカウントへ接続する Lightning ウォレットを設定します。以下を必ず確認:
- API key に "Read"、"Receive"、"Write" 権限があること（ないと Bolt Cards が資金を引き出せません）
- **Bitcoin wallet** に接続していること（*StableSats USD* ではない）

### 3. 自動払い出し設定

Bolt Cards への無操作トップアップを可能にするため、払い出しを自動処理する必要があります。

- "**Settings**" -> "**Payout Processors**" へ移動
- "**Automated Lightning Sender**" の下で "**Configure**" をクリック
- "**Process approved payouts instantly" を有効化
- "**Save**" ボタンをクリック

### 4. Bolt Cards Factory 設定

#### Bolt Card factory を設定

- 左サイドバーで "**Boltcard Factories**" へ移動
- "**App Name**": "Your conference" のような名前を入力（カード読み取り時に表示されます）
- "**Create**" ボタンをクリックし、次の設定を行います。例として 210 Sats を事前チャージする場合:
  - "**Name**": Bolt Card 読み取り時に表示するカンファレンス/イベント名
  - "**Amount**": 210
  - "**Currency**": SATS（必ず **SATS**。他通貨を設定しない）
  - "**Automatically approve claims**": チェックあり（true）
  - "**Payment Methods**": "BTC (Off-Chain)" にチェック（true）
  - "**Save**" ボタンをクリック

#### Bolt Cards をプログラム

同じ Boltcard Factory 設定ページで、右上の "**View**" ボタンをクリックします。開いたページは NFC 書き込み対応モバイル端末（例: Bitcoinize）で開く必要があります。

- モバイル端末に [Bolt Card NFC Card Creator](https://play.google.com/store/apps/details?id=com.lightningnfcapp&hl=en&gl=US) がインストールされていることを確認
- 既にインストール済みの場合は ***必ず最新バージョン*** であることを確認。必要ならアンインストール後、ストアから最新版を再インストール
- Boltcard Factory の "**Setup**" ボタンを押すと Bolt Card NFC Card Creator アプリが開くはずです
- Bolt Card を ***すべての緑チェックが完了するまで*** 端末にかざし続けてください
- その後は "**Write again**" ボタンでカードを連続初期化できます

### 5. カード残高確認とトップアップ

Bolt Card プロバイダ用ストアの左サイドバーに "**Boltcard Balance**" メニューが表示されます。URL はこの形式です: [https://btcpay.yourdomain.tld/boltcards/balance](https://btcpay.yourdomain.tld/boltcards/balance)

このリンクを NFC 対応モバイル端末（Bitcoinize など）で開くと、ユーザーが残高確認でき、さらに Sats でトップアップできます。トップアップするには、残高読み取り後に "**QR-Code icon**" をタップします。

### 6. カンファレンス/イベント後の Bolt Cards リセット

残高ページと同様、リンクは左サイドバーから確認できます。URL はこの形式です: [https://btcpay.yourdomain.tld/boltcards/balance?view=Reset](https://btcpay.yourdomain.tld/boltcards/balance?view=Reset)

このリンクはイベント中または事前に公開しておくことを検討してください。イベント後に参加者がカードを sweep して reset できるようにすると、Bolt Cards を再利用し再プログラムできます。これはリセット成功後のみ可能です。

Bolt Cards のリセットにも、セットアップ時と同じく Bolt Card NFC Card Creator アプリが必要です。

案内文の例として、[Uncle Rockstar Dev のこのツイート](https://x.com/r0ckstardev/status/1767618114139639817)を参照してください。

これで、参加者と出店者の双方にとってスムーズな体験を提供する、カンファレンス/イベント向け BTCPay Server セットアップが完了です。イベントを楽しんでください。
