
# モジュール管理
## go mod init [package]
go.mod（依存関係の記述）を生成。

## go mod tidy
依存関係の自動整理。
* コード内でimportしているがgo getされていないモジュールをダウンロード
* ダウンロードされているがコード内でimportされていないモジュールを削除
* 上記2つを実施したあとにgo.modとgo.sumを修正 または 削除

# 
## go:embed
* メリットはHTML、CSS、JavaScriptなどを単一バイナリに含めることができる。
