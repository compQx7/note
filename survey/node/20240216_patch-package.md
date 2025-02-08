# patch-package

- NPMパッケージの中身を直接修正し、その変更点（パッチ）を管理するためのツール。

## 使用例

仮のパッケージ "my-package" の中の何かを修正したいケース

まずはnode_modules/my-packageディレクトリに移動し、修正を加えます。

修正が終わったら、以下のコマンドを実行します。

```sh
npx patch-package my-package
```

patchesディレクトリが作成され、修正の差分がパッチとして保存されます。

package.jsonにpostinstallスクリプトを追加。

```json:package.json
"scripts": {
  // ...他のスクリプト
  "postinstall": "patch-package"
}
```

パッケージのインストールが終わる度に自動的に「patch-package」が起動し、作成したパッチが新たにインストールされたパッケージにも適用されるようになる。
