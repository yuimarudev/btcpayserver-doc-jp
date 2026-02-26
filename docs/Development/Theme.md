<!-- legacy-anchor-aliases -->
<span id="2-bootstrap-themes"></span>
<span id="checkout-page-theme"></span>
<span id="notes-on-bootstrap-css"></span>
<!-- /legacy-anchor-aliases -->

# テーマのカスタマイズ

BTCPay Server は Bootstrap をベースに構築されており、見た目を要件に合わせて柔軟に調整できます。
詳しくは [BTCPay で使われる標準デザイン仕様](https://design.btcpayserver.org/) を参照してください。

## カスタムテーマの開発と拡張

BTCPay Server のユーザーインターフェースは、[CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*) をサポートした **カスタマイズ版 Bootstrap** で構成されています。
これにより、[`bootstrap.css`](#notes-on-bootstrap-css) に影響を与えずに、フォントや色などテーマ関連の設定を変更できます。
また、テーマごとに `bootstrap.css` 全体を配布する代わりに、必要なカスタマイズ部分だけを提供できます。

このアプローチの概要は、[predefined themes](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer/wwwroot/main/themes/) を見ると理解しやすいです。

### 既存テーマの変更

`:root` セレクタ内のカスタムプロパティ定義は、カスケードとして捉えられる複数セクションに分かれています。

- 最初のセクションは汎用定義（例: ブランドカラーやニュートラルカラー）です。
- 2つ目のセクションは用途別の変数を定義します。
  ここでは汎用定義をマッピングしたり、追加の変数を作成したりできます。
- 3つ目のセクションはページ、セクション、コンポーネントなど特定部分向けの定義です。
  一貫した見た目にするため、可能な限り上位の定義を再利用してください。

テーマファイルで定義した変数は [`site.css`](https://github.com/btcpayserver/btcpayserver/blob/master/BTCPayServer/wwwroot/main/site.css) で使われます。

#### Bootstrap セレクタの上書き

変数に加えて、このファイルに **CSS セレクタを直接追加** してスタイルを指定することも可能です。
これは、変更したい対象に該当変数がない場合や軽微な微調整が必要な場合の最終手段として考えてください。

#### テーマ変数の追加

一般的には、特定用途（例: あるセクションのリンク色）向けに **専用変数** を導入するのが有効です。
こうすることで、汎用変数に結びついた他の部分へ影響を与えず、個別にスタイルを調整できます。

すべてのテーマで使う新しい変数を導入したい場合は、`site.css` に追加してください。
このファイルには Bootstrap スタイルへの変更内容が含まれます。
`bootstrap.css` を直接変更するのは避けてください。理由は [additional notes](#notes-on-bootstrap-css) を参照してください。

#### 新しいテーマの追加

既存の predefined themes のいずれかをコピーし、要件に合わせて変数を変更してください。

調整内容を試すには、ブラウザの開発者ツールも利用できます。
`<html>` 要素を検査し、スタイルインスペクタの `:root` セクションで変数を変更します。

多くの場合、次のように主要色を調整すれば十分です。

```css
:root {
  --btcpay-primary-100: #fef3e6;
  --btcpay-primary-200: #fcdcb5;
  --btcpay-primary-300: #fbc584;
  --btcpay-primary-400: #f9ae53;
  --btcpay-primary-500: #f79621;
  --btcpay-primary-600: #de7d08;
  --btcpay-primary-700: #ac6106;
  --btcpay-primary-800: #7b4504;
  --btcpay-primary-900: #4a2a03;

  --btcpay-primary-rgb: 247,150,33;
  --btcpay-primary-accent-rgb: 222, 125, 8;
  --btcpay-primary: rgb(var(--btcpay-primary-rgb));
  --btcpay-primary-accent: rgb(var(--btcpay-primary-accent-rgb));
  --btcpay-primary-shadow: rgba(var(--btcpay-primary-rgb), .5);
}
```

調整が終わったら、CSS をファイルとして保存し、`Server Settings > Branding` ページでアップロードします。

![CustomTheme](../img/BrandingTheme.png)

アップロードするとテーマが適用されます。
上記例では、カスタムテーマ適用後の表示は次のようになります。

![CustomTheme](../img/CustomTheme.png)
