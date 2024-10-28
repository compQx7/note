# peco & ghq

## 事前準備

go をインストール。

## ghq

ghq をインストール

```sh
go install github.com/x-motemen/ghq@latest
```

`/etc/profile`に ghq のパスを追加。

`~/.gitconfig`に下記を追加

```gitconfig
[ghq]
  root = <path>
```

## peco

pecoリポジトリをクローン。

リポジトリ内でビルド。

```sh
go build cmd/peco/peco.go
```

`/etc/profile`に peco のパスを追加。

## 基本的な使い方

```sh
ghq list
ghq root
ghq get <repository-url>
cd $(ghq list -p | peco)
ghq rm <dir>
ghq rm $(ghq list | peco)
```
