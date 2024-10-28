# ESLint
* カッコの数や文法エラー、未定義変数、セミコロン、フォーマットなど、ソースコードの不整合を自動で静的解析してくれる。
* ESLintの設定ファイルで検証項目をカスタマイズできる。
* 利用するにはnpmがインストールされていることが前提。
* 各種エディタの拡張機能を利用することで便利に導入できる。（VSCodeなら保存したタイミングで自動解析）
* ESLintにはグローバルインストールとローカルインストール（プロジェクト単位）がある。



# 導入手順

## nvm(Node Version Manager)インストール
例：nvm-windows（nvm-setup.zipをGithubから入手してインストール）

## npm(Node Package Manager)インストール
コマンドプロンプトなどで以下実行

インストール可能なNode.jsのバージョン表示
nvm list available

LTS（Long Term Support）の一番上のバージョンをインストール
nvm install 18.12.1

選択可能なNode.jsのバージョン表示
nvm list

バージョン切り替え（初回必須、管理者権限が必要）
nvm use 18.12.1

## npmの設定ファイル作成（プロジェクト単位のローカルインストールの場合）
コマンドプロンプトなどでプロジェクト直下に移動し、以下実行

npm init

## ESLintインストール（プロジェクト単位のローカルインストールの場合）
コマンドプロンプトなどでプロジェクト直下に移動し、以下実行

npm install -D eslint
※グルーバルインストールの場合は npm install -g eslint

## ESLintの設定ファイル作成
コマンドプロンプトなどでプロジェクト直下に移動し、以下実行

node node_modules/eslint/bin/eslint --init

生成された .eslintrc.js を自由にカスタマイズ

# 実行方法

## VSCodeで実行する場合
VSCodeで拡張機能ESLintをインストール
導入手順で用意した環境下であれば、VSCode再起動で自動的に検証される

## コマンドで実行する場合（プロジェクト直下から）
./node_modules/bin/eslint [PATH]
※PATHはディレクトリ指定でもファイル指定でも可
