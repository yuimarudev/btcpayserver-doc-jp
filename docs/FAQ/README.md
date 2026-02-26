<!-- legacy-anchor-aliases -->
<span id="btcpay-frequently-asked-questions-and-common-issues"></span>
<!-- /legacy-anchor-aliases -->

# よくある質問と一般的な問題

このドキュメントには、すべてのFAQと一般的な問題への目次が含まれています。

## [一般FAQ](./General.md)

BTCPayに関する一般的な非技術的な質問です。BTCPayとは何か、どのように動作するか、機能、なぜ他と異なるのか、誰が利用できるのかを説明します。

- [BTCPay Serverとは何ですか？](./General.md#what-is-btcpay-server)
- [なぜ他の決済プロセッサではなくBTCPayを選ぶべきですか？](./General.md#why-should-i-choose-btcpay-over-other-processors)
- [なぜ誰もがBTCPayにこれほど興奮しているのですか？](./General.md#why-is-everyone-so-excited-about-btcpay)
- [BTCPayは誰が利用できますか？](./General.md#who-can-use-btcpay)
- [BTCPayをインストールするには？](./General.md#how-to-install-btcpay-server)
- [BTCPayの動画チュートリアルはどこで見つけられますか？](./General.md#where-to-find-btcpay-video-tutorials)
- [BTCPay Serverを使うにはオンラインストアが必要ですか？](./General.md#do-i-need-to-have-an-online-store-to-use-btcpay-server)
- [購入者に公開アドレスを渡すだけではだめなのはなぜですか？](./General.md#why-cant-i-just-give-my-bitcoin-address-to-a-buyer)
- [BTCPayは請求書ごとにどのように新しいアドレスを作成しますか？](./General.md#how-does-btcpay-create-a-new-address-for-each-invoice)
- [BTCPayに秘密鍵は必要ですか？](./General.md#does-btcpay-need-myprivate-key)
- [BTCPay Serverは暗号資産から法定通貨への換算をサポートしていますか？](./General.md#does-btcpay-server-support-crypto-to-fiat-conversion)
- [BTCPay Serverの請求書の支払いで問題が起きたらどうすればよいですか？](./General.md#what-if-i-have-a-problem-paying-an-invoice)
- [支払い済み請求書に問題がある場合はどうすればよいですか？](./General.md#what-if-i-have-a-problem-with-a-paid-invoice)
- [ヘルプやサポートはどこで受けられますか？](./General.md#where-can-i-get-help-and-support)
- [BTCPayに貢献するには？](./General.md#how-can-i-contribute-to-btcpay)
- [BTCPay Server APIを使うには？](./General.md#how-can-i-use-the-btcpay-server-api)
- [BTCPay Serverをバックアップするには？](./General.md#how-can-i-backup-my-btcpay-server)
- [自分のBTCPay Serverインスタンスの利用料金を請求するには？](./General.md#how-can-i-charge-for-using-my-btcpay-server-instance)

## [デプロイFAQ](./Deployment.md)

BTCPayのインストールに関する質問と解決策です。

### [一般デプロイFAQ](./Deployment.md#general-deployment-faq)

- [BTCPay Serverの運用コストはいくらですか？](./Deployment.md#how-much-does-it-cost-to-run-btcpay-server)
- [BTCPayの最小要件は何ですか？](./Deployment.md#what-are-the-minimal-requirements-for-btcpay)
- [セルフホストのBTCPay Serverをデプロイする最も簡単な方法は何ですか？](./Deployment.md#what-is-the-easiest-method-to-deploy-a-self-hosted-btcpay-server)
- [適切なデプロイ方法を選ぶには？](./Deployment.md#how-to-choose-a-proper-deployment-method)
- [なぜVPSが必要なのですか？自宅のコンピューターでBTCPayを動かすだけではだめですか？](./Deployment.md#can-i-run-btcpay-on-my-home-computer)
- [自分のハードウェアでBTCPayを実行できますか？](./Deployment.md#can-i-run-btcpay-on-my-own-hardware)
- [既存のVPSにデプロイできますか？](./Deployment.md#can-i-deploy-btcpay-on-my-existing-vps)
- [試せる無料ホストはありますか？](./Deployment.md#are-there-free-hosts-where-i-can-test)
- [初回デプロイ後、登録できずログインもまだない場合は？](./Deployment.md#after-initial-deployment-i-can-t-register-and-i-don-t-have-a-login-yet)
- [BTCPay ServerでTorを有効にするには？](./Deployment.md#how-do-i-activate-tor-on-my-btcpay-server)
- [BTCPay ServerでTorを無効にするには？](./Deployment.md#how-do-i-disable-tor-on-my-btcpay-server)
- [Torを有効にする理由は？それは誰にも身元が分からないという意味ですか？](./Deployment.md#why-activate-tor-does-it-mean-that-nobody-knows-who-i-am)
- [clearnetなしで.onionアドレスにアクセスするには？](./Deployment.md#how-to-access-the-onion-address-without-clearnet)
- [環境変数を変更または無効化するには？](./Deployment.md#how-can-i-modify-or-deactivate-environment-variables)
- [BTCPayをtestnetで動かすには？](./Deployment.md#how-can-i-run-btcpay-on-testnet)
- [支払いを受ける予定があるときだけBTCPayを起動できますか？](./Deployment.md#can-i-start-btcpay-only-when-i-m-expecting-a-payment)
- [BTCPayのBitcoin P2Pにポート8333で接続できますか？](./Deployment.md#can-i-connect-to-my-btcpay-bitcoin-p2p-on-port-8333)
- [SSL証明書を更新するには？](./Deployment.md#how-can-i-renew-my-ssl-certificate)
- [既存のNginxサーバーをSSL終端付きリバースプロキシとして使えますか？](./Deployment.md#can-i-use-an-existing-nginx-server-as-a-reverse-proxy-with-ssl-termination)
- [Bitcoin Coreの代わりにBitcoin Knotsを使えますか？](./Deployment.md#can-i-use-bitcoin-knots-instead-of-bitcoin-core)

### [WebデプロイFAQ](./Deployment.md#web-deployment-faq)

#### [Luna Node WebデプロイFAQ](./Deployment.md#luna-node-web-deployment-faq)

- [LunaNode BTCPayのドメイン名を変更するには？](./Deployment.md#how-to-change-domain-name-on-my-lunanode-btcpay)

### [手動デプロイFAQ](./Deployment.md#manual-deployment)

- [Ubuntu 18.04にBTCPayを手動インストールするには？](./Deployment.md#how-to-manually-install-btcpay-on-ubuntu-18-04)
- [linux環境（docker版）からBTCPayを完全にアンインストールするには？](./Deployment.md#how-do-i-completely-uninstall-btcpay-from-a-linux-environment-docker-version)
- [既存のBitcoinフルノードと並行してBTCPay Serverをデプロイするには？](./Deployment.md#how-to-deploy-btcpay-server-alongside-existing-bitcoin-node)
- [dockerデプロイで、データ用に別のボリュームを使うには？](./Deployment.md#with-the-docker-deployment-how-to-use-a-different-volume-for-the-data)
- [ローカルサーバーでhttpsとhttpに500 nginxエラーが出る（BTCPay is expecting you to access this website from）](./Deployment.md#cause-4-getting-500-nginx-error-on-a-local-server-https-and-for-http-btcpay-is-expecting-you-to-access-this-website-from)
- [エラー: BTCPay is expecting you to access this website from...](./Deployment.md#cause-3-btcpay-is-expecting-you-to-access-this-website-from)
- [安全でないネットワーク経由でBTCPay Serverにアクセスしている](./Deployment.md#cause-3-btcpay-is-expecting-you-to-access-this-website-from)

## [同期FAQ](./Synchronization.md)

BTCPayの初回同期中に発生しうる一般的な質問と問題です。

- [なぜBTCPayは同期するのですか？](./Synchronization.md#why-does-btcpay-sync)
- [同期をスキップまたは高速化できますか？](./Synchronization.md#can-i-skip-the-synchronization)
- [同期が完了したことをどう確認できますか？](./Synchronization.md#how-do-i-know-that-btcpay-synced-completely)
- [自分のbitcoinノードのブロック高を確認するには？](./Synchronization.md#how-can-i-check-the-block-height-of-my-bitcoin-node)
- [BTCPayの同期に非常に時間がかかる](./Synchronization.md#btcpay-server-takes-forever-to-synchronize)
- [BTCPay Serverでノードが常に起動中と表示され続ける](./Synchronization.md#btcpay-server-keeps-showing-that-my-node-is-always-starting)
- [すでに同期済みのフルノードがあります。BTCPayで使えますか？](./Synchronization.md#im-running-a-full-node-and-have-a-synched-blockchain-can-btcpay-use-it-so-that-it-doesnt-have-to-do-a-full-sync)
- [Bitcoinノードのpruningを有効にするには？](./Synchronization.md#how-to-enable-bitcoin-node-pruning)
- [Bitcoinノードのpruningを無効にするには？](./Synchronization.md#how-to-disable-bitcoin-node-pruning)

## [連携FAQ](./Integrations.md)

ECサイトなどの連携に関する質問です。

### [連携の一般FAQ](./Integrations.md#integrations-general-faq)

- [利用可能なeコマース連携は何がありますか？](./Integrations.md#what-e-commerce-integrations-are-available)
- [BTCPayにShopifyプラグインはありますか？](./Integrations.md#does-btcpay-have-a-shopify-plugin)
- [連携なしでBTCPayを使えますか？](./Integrations.md#can-i-use-btcpay-without-an-integration)

### [WooCommerce FAQ](./Integrations.md#woocommerce-faq-2)

- [WooCommerceで注文ステータスを設定するには？](./Integrations.md#how-to-configure-order-status-in-woocommerce)
- [WooCommerceでメール確認をカスタマイズするには？](./Integrations.md#how-to-customize-e-mail-confirmations-in-woocommerce)
- [エラー: 別の注文番号システムを使う場合は class-wc-gateway-btcpay.php を参照して検索フィルターを適用してください](./Integrations.md#error-if-you-use-an-alternative-order-numbering-system-please-see-class-wc-gateway-btcpayphp-to-apply-a-search-filter)

## [サーバー設定FAQ](./ServerSettings.md)

サーバー管理者が抱える一般的な問題と質問です。

### [メンテナンスFAQ](./ServerSettings.md#maintainance)

- [BTCPay Serverを更新するには？](./ServerSettings.md#how-to-update-btcpay-server)
- [BTCPay Serverを再起動するには？](./ServerSettings.md#how-to-restart-btcpay-server)
- [VPS上で動作中のBTCPayにSSH接続するには？](./ServerSettings.md#how-to-ssh-into-my-btcpay-running-on-vps)
- [BTCPay Serverのバージョンを確認するには？](./ServerSettings.md#how-can-i-see-my-btcpay-version)
- [ターミナル経由でBTCPay Serverのバージョンを確認するには？](./ServerSettings.md#how-can-i-check-my-btcpay-server-version-via-terminal)
- [BTCPay SSH鍵ファイルとは何ですか](./ServerSettings.md#what-is-btcpay-ssh-key-file)
- [BTCPay管理者パスワードを忘れた](./ServerSettings.md#forgot-btcpay-admin-password)
- [招待で新しいユーザーを追加するには？](./ServerSettings.md#how-to-add-a-new-user-by-invite)
- [ユーザーのU2Fと2FAを無効化するには？](./ServerSettings.md#how-to-disable-u2f-and-2fa-for-a-user)
- [BTCPayでSMTP設定を構成するには？](./ServerSettings.md#how-to-configure-smtp-settings-in-btcpay)
- [エラー: メンテナンス機能にはBTCPayServer設定で正しく構成されたSSHアクセスが必要です](./ServerSettings.md#error-maintenance-feature-requires-access-to-SSH-properly-configured-in-btcpayserver-configuration)
- [エラー: ローカル変更により以下のファイルがマージで上書きされます](./ServerSettings.md#error-your-local-changes-to-the-following-files-would-be-overwritten-by-merge)
- [エラー: BTCPAY_SSHKEYFILE変数が未設定 / 更新できない](./ServerSettings.md#error-btcpay-sshkeyfile-is-not-set-when-running-the-docker-install-or-unable-to-update-through-server-settings-maintenance)

### [テーマ / カスタマイズFAQ](./ServerSettings.md#theme-customization)

- [BTCPayテーマスタイルをカスタマイズするには？](./ServerSettings.md#how-to-customize-my-btcpay-theme-style)
- [BTCPayのチェックアウトページを変更するには？](./ServerSettings.md#how-to-modify-the-checkout-page)
- [POSアプリのテーマをカスタマイズするには？](../Development/Theme.md#2-bootstrap-themes)
- [BTCPayにGoogle Analyticsコードを追加するには？](./ServerSettings.md#how-to-add-google-analytics-code-to-btcpay)

### [ポリシーFAQ](./ServerSettings.md#policies)

- [BTCPay Serverで登録を許可するには？](./ServerSettings.md#how-to-allow-registration-on-my-btcpay-server)
- [検索エンジンからBTCPay Serverを隠すには？](./ServerSettings.md#how-to-hide-my-btcpay-server-from-search-engines)

### [サービスFAQ](./ServerSettings.md#services)

- [BTCPayフルノードへリモート接続するには？](./ServerSettings.md#how-to-remotely-connect-to-my-btcpay-full-node)

### [ファイルFAQ](./ServerSettings.md#files)

- [BTCPayへファイルをアップロードするには？](./ServerSettings.md#how-to-upload-files-to-btcpay)

## [ストアFAQ](./Stores.md)

ストア設定の説明です。

- [BTCPayでストアを作成するには？](./Stores.md#how-to-create-a-store-in-btcpay)
- [ストアはいくつ作成できますか？](./Stores.md#how-many-stores-can-i-create)
- [支払いのない請求書が完了として表示されるのはなぜですか？](./Stores.md#why-are-invoices-without-payment-showing-as-complete)
- [ストアの一般設定](./Stores.md#store-general-settings)
- [請求書にネットワーク手数料を追加する（採掘手数料で変動）には？](./Stores.md#add-network-fee-to-invoice-vary-with-mining-fees)
- [誰でも請求書を作成できるようにするには？](./Stores.md#allow-anyone-to-create-invoice)
- [全額が支払われない場合、請求書を...分後に失効させるには？](./Stores.md#invoice-expires-if-the-full-amount-has-not-been-paid-after-minutes)
- [請求書期限切れ後...分経ってもトランザクションが承認されない場合、支払いを無効にするには？](./Stores.md#payment-invalid-if-transactions-fails-to-confirm-minutes-after-invoice-expiration)
- [請求書を支払いトランザクション時点で承認済みと見なすには？](./Stores.md#consider-the-invoice-confirmed-when-the-payment-transaction)
- [支払い額が想定より...%少なくても請求書を支払い済みと見なすには？](./Stores.md#consider-the-invoice-paid-even-if-the-paid-amount-is-less-than-expected)
- [請求書でメールを無効化するには？](./Stores.md#how-to-disable-email-on-invoices)
- [請求書をsats建てにするには？](./Stores.md#how-to-denominate-invoices-in-sats)
- [購入者の追加情報を収集するには？](./Stores.md#how-to-collect-additional-buyer-information)
- [支払い後にストア請求書をリダイレクトするには？](./Stores.md#how-to-redirect-store-invoices-after-payment)
- [BTCPayから請求書を削除できますか？](./Stores.md#can-i-delete-invoices-from-btcpay)
- [請求書の為替レートプロバイダーを変更するには？](./Stores.md#how-to-change-the-exchange-rate-provider-for-invoices)
- [GetRatesAsync was called on coinaverage エラーが出る](./Stores.md#getting-getratesasync-was-called-on-coinaverage-error)

## [ウォレットFAQ](./Wallet.md)

BTCPayのウォレットに関する質問と問題です。

- [BTCPay Serverウォレットとは何ですか？](./Wallet.md#what-is-btcpay-server-wallet)
- [BTCPay Serverでウォレットを設定するには？](./Wallet.md#how-to-set-up-my-wallet-with-btcpay-server)
- [BTCPay Serverでハードウェアウォレットを使えますか？](./Wallet.md#can-i-use-a-hardware-wallet-with-btcpay-server)
- [BTCPay Serverウォレットを使う必要がありますか？](./Wallet.md#do-i-have-to-use-btcpay-server-wallet)
- [Trezorでトランザクション送信が失敗するのはなぜですか？](./Wallet.md#why-is-sending-a-transaction-using-trezor-failing)
- [ウォレットで支払いが見つからない？](./Wallet.md#missing-payments-in-my-software-or-hardware-wallet)
- [導出スキームとは何ですか？](./Wallet.md#what-is-a-derivation-scheme)
- [Replace-By-Fee (RBF) トランザクションとは何ですか？](./Wallet.md#what-is-a-replace-by-fee-rbf-transaction)
- [トランザクションにカスタムラベルやコメントを追加するには？](./Wallet.md#how-to-add-custom-labels-and-comments-to-transactions)
- [BTCPayウォレットでLightning networkの支払いが表示されない？](./Wallet.md#i-dont-see-lightning-network-payments-in-btcpay-wallet)
- [BTCPay Serverウォレット用のモバイルアプリはありますか？](./Wallet.md#is-there-a-mobile-app-for-btcpay-server-wallet)

## [アプリFAQ](./Apps.md)

BTCPayのアプリケーションに関するよくある質問です。

- [BTCPayのアプリとは何ですか？](./Apps.md#what-are-the-apps-in-btcpay)
- [作成できるアプリ数に上限はありますか？](./Apps.md#is-there-a-limit-on-the-number-of-apps-i-can-create)
- [BTCPayにPoint of Sale機能はありますか？](./Apps.md#is-there-a-point-of-sale-feature-in-btcpay)
- [実店舗でBTCPayを使うには？](./Apps.md#how-can-i-use-btcpay-in-a-physical-store)
- [BTCPayでPOSの外観をカスタマイズするには？](./Apps.md#how-to-customize-the-appearance-of-Point-of-Sale-App-in-BTCPay)
- [Payment Buttonとは何ですか？](./Apps.md#what-is-a-payment-button)
- [カスタム金額のPay Buttonを作成するには？](./Apps.md#how-to-create-a-pay-button-with-a-custom-amount)
- [ドメイン名をアプリに紐づけるには？](./Apps.md#how-to-map-a-domain-name-to-an-app)
- [支払い後に別サイトへリダイレクトするには？](./Apps.md#how-to-redirect-to-another-site-after-payment)
- [WooCommerceストアをBTCPay Crowdfundアプリに統合するには？](./Apps.md#how-to-integrate-woocommerce-store-into-a-btcpay-crowdfund-app)

## [Lightning Network FAQ](./LightningNetwork.md)

Lightning Networkのトラブルシューティングと一般的な問題です。

### [Lightning Network 一般FAQ](./LightningNetwork.md#lightning-network-general-faq)

- [BTCPayでLightning Networkを利用できるユーザー数は？](./LightningNetwork.md#how-many-users-can-use-lightning-network-in-btcpay)
- [BTCPayを使うストアのノード情報を見つけて直接チャネルを開くには？](./LightningNetwork.md#how-to-find-node-info-and-open-a-direct-channel-with-a-store-using-btcpay)
- [加盟店として、直接チャネルを開く必要がありますか？](./LightningNetwork.md#as-a-merchant-do-i-need-to-open-direct-channels)
- [ノードの受信キャパシティを得るには？](./LightningNetwork.md#how-can-i-get-inbound-capacity-to-my-node)
- [以前LightningなしでBTCPayをインストールしました。今から有効化できますか？](./LightningNetwork.md#i-previously-installed-btcpayserver-without-lightning-can-i-enable-it)
- [BTCPayのLNでprunedノードは使えますか？](./LightningNetwork.md#can-i-use-a-pruned-node-with-ln-in-btcpay)
- [既存のLNノードをBTCPayで使えますか？](./LightningNetwork.md#can-i-use-my-existing-ln-node-with-btcpay)
- [Core Lightning (CLN) と LND を切り替えるには？](./LightningNetwork.md#how-to-change-from-core-lightning-cln-to-lnd-or-vice-versa)
- [Lightning Network実装を切り替えたら "no payment available" エラーが出る](./LightningNetwork.md#i-switched-lightning-network-implementation-but-getting-no-payment-available-error)
- [コンテナ起動時に "WARNING: The LIGHTNING_ALIAS variable is not set. Defaulting to a blank string" と表示される](./LightningNetwork.md#i-get-warning-the-lightning-alias-variable-is-not-set-defaulting-to-a-blank-string-when-starting-container)
- [他者が接続できるようにLightning Node情報を表示するには？](./LightningNetwork.md#how-to-display-my-lightning-node-information-so-that-others-can-connect-to-me)
- [BTCPay ServerでLightning Networkウォレットのリカバリーシードバックアップはどこにありますか？](./LightningNetwork.md#where-can-i-find-recovery-seed-backup-for-my-lightning-network-wallet-in-btcpay-server)
- [オンチェーン支払いを無効にしてLN支払いのみを使うには？](./LightningNetwork.md#how-to-disable-on-chain-payments-and-use-ln-payments-only)
- [Lightning Networkのサポートはどこで受けられますか？](./LightningNetwork.md#lightning-network-questions-and-support)
- [Lightning Networkのバージョンを確認するには？](./LightningNetwork.md#how-to-see-my-lightning-network-version)

### [Lightning Network LND FAQ](./LightningNetwork.md#lightning-network-lnd-faq)

- [LNDを再起動するには？](./LightningNetwork.md#how-to-restart-my-lnd)
- [LNDログを確認するには？](./LightningNetwork.md#how-to-see-lnd-logs)
- [BTCPayにおけるLNDのデフォルトディレクトリは？](./LightningNetwork.md#what-s-the-default-directory-of-lnd-in-btcpay)
- [外部ノード接続に必要なmacaroonは？](./LightningNetwork.md#which-macaroon-needs-to-be-provided-for-external-nodes)
- [LND接続問題 - cannot get macaroon: root key with id 0 doesn’t exist](./LightningNetwork.md#lnd-connection-issues-after-an-update)
- [LNDノードのエイリアスを変更するには](./LightningNetwork.md#how-to-change-my-LND-Node-alias)
- [lnd.confを編集するには](./LightningNetwork.md#how-to-edit-lndconf)
- [ThunderHubをインストールするには](./LightningNetwork.md#how-to-install-thunderhub)

### [Lightning Network Core Lightning (CLN) FAQ](./LightningNetwork.md#lightning-network-core-lightning-cln-faq)

- [Core Lightning (CLN) を再起動するには？](./LightningNetwork.md#how-to-restart-my-core-lightning-cln)
- [IPv6アドレスをアナウンスするには？](./LightningNetwork.md#how-to-announce-an-ipv6-address)
- [.lightning/configを編集するには](./LightningNetwork.md#how-to-edit-lightningconfig)

## [アルトコインFAQ](./Altcoin.md)

- [BTCPay Serverはどのコインをサポートしていますか？](./Altcoin.md#which-coins-does-btcpay-server-support)
- [BTCPayにXYZコインを追加できますか？](./Altcoin.md#can-an-xyz-coin-be-added-in-btcpay)
- [BTCPayにアルトコインを追加するには？](./Altcoin.md#how-to-add-an-altcoin-in-btcpay)
- [既存のBTCPayデプロイにアルトコインを追加するには？](./Altcoin.md#how-to-add-an-altcoin-to-an-existing-btcpay-deployment)
- [BTCPayからコインを削除するには？](./Altcoin.md#how-to-remove-a-coin-from-btcpay)
