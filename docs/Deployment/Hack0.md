# Hack0

Hack0 は [Raspberry Pi](/Deployment/RaspberryPi4.md) デプロイの代替です。
ハードウェアと OS が提供されるため、docker デプロイで BTCPay Server を動かすためのインストール手順を大幅に簡素化できます。

このプロジェクトは [DG Lab](https://www.dglab.com/en/) がメンテナンスしています。サポートが必要な場合は [support chat](https://chat.btcpayserver.org/btcpayserver/channels/hack0) に参加してください。

Hack0 は2種類のユーザーを想定しています: `Distributors` と `End users`。

- `End users` は最終的に自分の用途で Hack0 を運用する人です。
- `Distributors` は各種ハードウェア部品を購入して組み立て、Hack0 をインストールし、`End users` 向けにプラグアンドプレイの箱として配布する人です。

Hack0 の部品を自分で購入・組み立て・インストールして自分で使う場合、このドキュメント上では `distributor` と `end users` の両方に該当します。

紹介動画はこちらです。

[![Introduction to Hack0](https://img.youtube.com/vi/m3i2EUTEukM/mqdefault.jpg)](https://www.youtube.com/watch?v=m3i2EUTEukM)

## ハードウェア仕様（distributors 向け）

Hack0 の動作に推奨されるパーツは次のとおりです。

- RockPro64 4GB ([リンク](https://store.pine64.org/?product=rockpro64-4gb-single-board-computer)) `79.99$`
- EMMC Module 用 USB アダプター ([リンク](https://pine64.com/product/usb-adapter-for-emmc-module/)) `4.99$`
- EMMC 32GB ([リンク](https://pine64.com/product/32gb-emmc-module/)) `24.95$`
- ROCKPro64 20mm Mid Profile Heatsink 用ファン ([リンク](https://pine64.com/product/fan-for-rockpro64-20mm-mid-profile-heatsink/)) `2.99$`
- ROCKPro64 20mm Mid Profile Heatsink ([リンク](https://pine64.com/product/rockpro64-20mm-mid-profile-heatsink/)) `3.29$`
- SSD 500GB PCIe NVMe ([リンク](https://www.crucial.com/ssd/p2/CT500P2SSD8)) `66.99$`
- M.2 to PCIe アダプター ([リンク](https://www.silverstonetek.com/en/product/info/expansion-cards/ECM25/)) `25$`

合計: `188.2$`

EMMC モジュールとアダプターを microSD に置き換えることも可能です。ただしテストでは、microSD は長期運用で信頼性が低く、2〜3年の使用後に動作しなくなる可能性が示されています。

## 出荷時インストール（distributors 向け）

ハードウェアを用意したら、Hack0 イメージを書き込みます。

Hack0 は armbian ディストリビューションをベースにしています。[GitHub ページの手順](https://github.com/dgarage/hack0-armbian/tree/btcpay/userpatches)に従って自分でイメージをビルドできます。時間を節約したい場合は、同ページにあるダウンロード可能な [pre-built image](https://github.com/dgarage/hack0-armbian/tree/btcpay/userpatches#pre-built-images) も利用できます。

イメージを入手したら、EMMC Module 用 USB アダプターを使って EMMC モジュールに書き込みます。
初回起動時、hack0 は `setup mode` で動作し、このセットアップモードでは次が行われます。

> :warning: pre-built image を最初に起動すると、hack0 は `setup mode` となり、SSD ドライブ上のデータをすべて消去します。

`setup mode` 中は ethernet コネクタ横の2つの LED を確認してください。赤色 LED は点灯したまま、白色 LED は点滅します。
セットアップが成功すると、赤色 LED は消灯し、白色 LED は点滅せずに点灯します。この時点で Hack0 を安全に取り外せます。`end users` が利用できる状態です。

セットアップに失敗した場合は、赤色 LED が点灯し、白色 LED は消灯します。

## エンドユーザー向けセットアップ

エンドユーザーは、hack0 を ethernet ケーブルでネットワークに接続するだけです。
5分ほど待つと `http://hack0.local` にアクセスでき、BTCPay Server インスタンスの登録フォームが表示されます。

場合によっては `hack0.local` が機能せず、[Angry IP Scanner](https://angryip.org/) のようなツールで hack0 の IP アドレスを探して接続する必要があります。ルーターに管理画面がある場合はそこでも hack0 の IP を確認できます。その場合は `http://<ipaddress>` に接続してください。

## FAQ

### hack0 に SSH で接続するには？

`http://hack0.local/server/services/ssh` に公開 ssh キーを追加する必要があります。すでに存在する `btcpayserver` キーは削除しないでください。
これで `ssh root@hack0.local` または Putty で ssh 接続できるはずです。

![SSH Authorized keys](../img/SSH-Authorized-Keys.png)
