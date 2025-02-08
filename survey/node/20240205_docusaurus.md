# [Docusaurus](https://docusaurus.io/)

## 参考サイト

[設定の編集](https://engineerblog.mynavi.jp/technology/make_doc_md/)
[何ができるか](https://zenn.dev/ningensei848/articles/docusaurus_intro)
[blog](https://griponminds.jp/blog/docusaurus-01/)

[docusaurus-search-local](https://github.com/easyops-cn/docusaurus-search-local)

## サイト構築手順

1. プロジェクトの作成

    ```sh
    npx create-docusaurus@latest src/website classic
    ```

2. 余分なファイルを削除
3. カスタマイズ

下記コマンドでビルドすると`src/ui/scripts/generate-doc-site/src/website/build`に展開されます。

```sh
npm run build
```

ホスティング例

```sh
npx http-server -p 8080 ./build
```

## カスタマイズ

### サイト設定

`src/ui/scripts/generate-doc-site/src/website/docusaurus.config.js`を編集します。

URL（ホスト）、BaseURL（ホスト以降）、ナビゲーションバーの編集もこのファイルになります。

### サイト全体のStyle

`src/ui/scripts/generate-doc-site/src/website/src/css/custom.css`を編集します。

[カラーテーマ プレビュー](https://docusaurus.io/docs/styling-layout#styling-your-site-with-infima)

### サイドバーの編集

`src/ui/scripts/generate-doc-site/src/website/sidebars.js`を編集します。

サイドバーに新たに項目を追加する場合は、devDocsSidebarプロパティ（配列）に項目を追加します。  
dirNameは`src/ui/scripts/generate-doc-site/src/website/docs`をルートとしたパスを記述します。

サイドバーの並び順は、各階層の`_category_.json`のposition値や、各ファイル先頭に記述できるsidebar_position値の大小によって制御できます。

[サイドバー](https://docusaurus.io/docs/sidebar)

### プラグインの追加

`src/ui/scripts/generate-doc-site/src/website/docusaurus.config.js`の`plugins`に追加する。

[プラグイン](https://docusaurus.io/docs/advanced/plugins)
