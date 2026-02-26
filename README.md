# BTCPay Server ドキュメント

[![Build Status](https://github.com/btcpayserver/btcpayserver-doc/workflows/Build/badge.svg)](https://github.com/btcpayserver/btcpayserver-doc/actions/workflows/build.yml)

## 紹介

BTCPay Server は、Bitcoin とその他の暗号通貨向けのオープンソースかつセルフホスト型の決済プロセッサです。

BTCPay Server の利用で問題がある場合は、コミュニティメンバーからサポートを受けるために、[公式サイトに掲載されているコミュニティ](https://btcpayserver.org/#communityCTA)への参加を検討してください。

ほかのチャネルで解決できない技術的な問題、またはコミュニティの他メンバーと検証済みの機能要望のみを [Github issue](https://github.com/btcpayserver/btcpayserver/issues) に提出してください。

詳細は、[公式サイト](https://btcpayserver.org/)、[完全なドキュメント](https://docs.btcpayserver.org/)、および [FAQ](https://docs.btcpayserver.org/FAQ/) をご確認ください。

## 貢献

Pull request は歓迎されており、高く評価されます。BTCPay Server に貢献する前に、まず [contributing guidelines](docs/Contribute/README.md) を確認してください。

初心者の方は、以下の BTCPay Server ドキュメントへの貢献手順ガイドを確認してください。

[![Contributing to Documentation](https://img.youtube.com/vi/bSDROcdSSWw/mqdefault.jpg)](https://www.youtube.com/watch?v=bSDROcdSSWw)

### ドキュメントをローカルでビルドする

Web サイトをローカルでビルドするには、[Node.js](https://nodejs.org/) >= 12.16（基本的には最新の LTS 版）が必要です。
`setup-deps.sh` スクリプトの前提条件として [jq](https://stedolan.github.io/jq/) が必要です。

セットアップはシンプルです。

```bash
# Install dependencies
npm install

# Link external doc repos
./setup-deps.sh

# Serve locally (by default on port 8080)
npm start
```

### テキストハイライト

異なる色のボックスを表示するために、[3 種類のテキストハイライト](https://vuepress.vuejs.org/guide/markdown.html#custom-containers) を利用できます。

親しみやすいヒントを表示する緑色のボックス:

```md
:::tip
foo
:::
```

注意喚起を表示する黄色のボックス:

```md
:::warning
foo
:::
```

明確な危険を示す赤色のボックス。任意のコンテナにはタイトル `foo` を追加できます:

```md
:::danger foo
bar
:::
```

### SEO の改善

サイト全体と各ページに関連するメタタグを追加するために、[Vuepress SEO plugin](https://www.npmjs.com/package/vuepress-plugin-seo) を使用しています。

特定ページのメタ属性を改善するには、次のように YAML frontmatter を追加できます（例として WooCommerce ページを参照してください）。

```text
---
description: How to integrate BTCPay Server into your WooCommerce store.
tags:
- WooCommerce
- WordPress
- Plugin
- eCommerce
---
# WooCommerce integration

This document explains how to **integrate BTCPay Server into your WooCommerce store**.
```

### YouTube 動画の埋め込み

プレビュー付きで YouTube 動画を追加するには、次のようにリンクします。

```md
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/mqdefault.jpg)](https://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)
```

埋め込み動画として表示されるには、リンク項目がプレビュー画像（YouTube 由来またはカスタム画像）である必要があります。

### 外部ドキュメントリポジトリ

このドキュメントサイトのビルドでは、複数リポジトリに存在するドキュメントを組み合わせています。

ビルド前に他リポジトリをチェックアウトし、適切な場所にドキュメントをコピーして、このリポジトリ内のドキュメントと同様にリンクします。

この処理は [setup-deps.sh](./setup-deps.sh) スクリプトで定義されています。

外部リポジトリは、変更時にドキュメントビルドをトリガーできます。
そのために、API 経由で利用できる GitHub の [repository_dispatch](https://help.github.com/en/actions/reference/events-that-trigger-workflows#external-events-repository_dispatch) 機能を使えます。

```sh
curl -X POST https://api.github.com/repos/btcpayserver/btcpayserver-doc/dispatches \
  -u "${{ secrets.GH_PAT }}" \
  -H "Accept: application/vnd.github.everest-preview+json" \
  -H "Content-Type: application/json" \
  --data '{"event_type": "build_docs"}'
```

`GH_PAT` は [personal access token](https://help.github.com/en/actions/reference/events-that-trigger-workflows#triggering-new-workflows-using-a-personal-access-token) である必要があります。

## サポーター

BTCPay Server プロジェクトは、[BTCPay Server Foundation](https://foundation.btcpayserver.org/) を通じて、以下の組織から誇りを持って支援を受けています。

[![Spiral](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_spiral.svg)](https://spiral.xyz)
[![OpenSats](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_opensats.svg)](https://opensats.org)
[![Tether](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_tether.svg)](https://tether.to)
[![Human Rights Foundation](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_hrf.svg)](https://hrf.org)
[![LunaNode](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_lunanode.svg)](https://lunanode.com)
[![Wallet of Satoshi](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_walletofsatoshi.svg)](https://walletofsatoshi.com/)
[![Coincards](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_coincards.svg)](https://coincards.com/)
[![IVPN](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_ivpn.svg)](https://ivpn.net/)
[![Unbank](https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/readme/supporter_unbank.svg)](https://unbank.com/)

プロジェクトを支援したい場合は、[donation page](https://btcpayserver.org/donate/) をご覧ください。
