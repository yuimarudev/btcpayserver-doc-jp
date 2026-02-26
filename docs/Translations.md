# BTCPay Translation 機能を使って BTCPay Server インスタンスをローカライズする

BTCPay Server 2.0 以降では、インスタンスにアクセスするユーザー向けのデフォルト言語を管理者が設定できる翻訳機能が含まれています。

この機能により、バックオフィス全体のデフォルト英語テキストを任意の言語に置き換えられます。

ここでは、BTCPay server を使いやすくするための翻訳の作成と管理方法を紹介します。

## 言語パック

BTCPay Server 2.3 以降では、翻訳をゼロから作成する代わりに、コミュニティ投稿の言語パックをダウンロードできます。言語パックを使うと、コミュニティが提供した翻訳を使ってサーバーのバックエンド全体をすばやくローカライズできます。

[![BTCPay Language Packs](https://img.youtube.com/vi/WKxcdVIDT04/mqdefault.jpg)](https://www.youtube.com/watch?v=WKxcdVIDT04)

### 言語パックのダウンロード

- Server Settings >> `Translation` に移動します
- `Download language pack` をクリックします
- ドロップダウンから言語を選択します
- `Download` をクリックします
ダウンロード後、言語パックは辞書一覧に表示されます。

#### 言語パックの適用

- `Select` をクリックして、ダウンロードした言語パックをサーバーのデフォルト言語に設定します

#### ダウンロードした言語パックの編集

ダウンロードした言語パックをカスタマイズしたい場合は、まず辞書をクローンすることを推奨します。  
変更はクローンしたバージョンに適用し、それをアクティブ辞書として選択してください。これにより、将来オリジナルの言語パックが更新されても、カスタム変更が上書きされるのを防げます。

希望する言語が利用できない場合でも、新しい辞書を手動で作成し、[submit your translation](https://github.com/btcpayserver/btcpayserver-translator) して他の人に役立ててもらうことができます。

## BTCPay Server を翻訳する

1. BTCPay Server インスタンスにログインします。
2. **Server Settings** >> **Translations** の Translation に移動します。

![Translation 1](./img/Translations/01_Translation.png)

3. **Create** ボタンをクリックして、新しい言語辞書を作成します。

4. 辞書名を入力し、**Create** ボタンをクリックして辞書を作成します。

![Translation 2](./img/Translations/02_Translation.png)

![Translation 3](./img/Translations/03_Translation_creation.png)

![Translation 4](./img/Translations/04_Translation_dictionary.png)


上の画像では、BTCPay Server 上で翻訳できる語句の辞書を確認できます。

辞書は通常、`"key": "value"` のペアで構成されます。ここで:

**Key:** BTCPay インスタンス内の元の英語テキストまたはフレーズ。

**Value:** 表示したい翻訳テキスト。

各英語の用語について、対応するテキスト欄に選択した言語の訳を入力します。正確さと明確さを確認してください。

例として、"Add Role" を Yoruba に翻訳してみます。Yoruba 用の辞書を作成したので、Yoruba で翻訳を入力します。

![Translation 5](./img/Translations/05_Translation_Add_Role_To_Yoruba.png)

テキストを置き換えて **Save** ボタンをクリックします。翻訳が正常に保存されたことを示す確認メッセージが表示されます。次に、新しい辞書の **Select** ボタンをクリックして、システムのデフォルトに設定します。

![Translation 6](./img/Translations/06_Translation_Saved_Dictionary.png)

翻訳をテストしてみましょう。**Server Settings** 配下の **Roles** に移動し、画面右上のボタンを確認すると、"Add Roles" が "Fi ipa kun" に翻訳されていることが分かります。

![Translation 7](./img/Translations/07_Translation_Validation.png)

BTCPay アプリ内で "Add Role" が表示される他の場所も検索し、選択した言語に正しく翻訳されていることを確認できます。

1つ翻訳できたら、辞書内の他のテキストも翻訳してください。英語テキストのすべての出現箇所が翻訳版に置き換わり、ローカライズされた体験を利用できるようになります。

:::tip
翻訳文字列は 1 つのテキストボックスにまとまっているため、簡単にコピーできます。この特性を活かして ChatGPT などの翻訳 AI ツールに貼り付け、初期案を作成することができます。AI 依存では文脈が失われる場合があるため、最終的にはすべての文字列を手動で確認し、文脈に沿った正確性を担保することを強く推奨します。
:::

## 効果的な翻訳のヒント

- **一貫性が重要:** 類似する用語は全体を通して一貫して翻訳してください。
- **文脈が重要:** フレーズを翻訳する際は文脈を意識して意味を保ってください。
- **定期的な見直し:** 新機能の追加に合わせて翻訳を定期的に更新してください。
