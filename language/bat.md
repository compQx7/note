# 変数

環境変数一覧表示
set

変数クリア
set var1=

変数にセット
set var1=100

変数を参照
echo %car1%

setlocal と endlocal で挟んだ間の変数はローカル変数となる。
挟んでいない場合は、グルーバル変数となる。


# コメント
rem


# ラベル
gotoやcallの呼び出し先として定義できる。
:label1


# 外部呼出し
call [Command/Batch/Label]
外部コマンドや外部バッチを呼び出す。
ラベルと組み合わせてサブルーチンを実現可能

start
callと違い、別コマンドプロンプトが別プロセスで立ち上げる。
/D 別ウィンドウのカレントディレクトリを指定する
/WAIT 別コマンドが終了するのを待つ


# 参考
[とほほのバッチ入門](https://www.tohoho-web.com/ex/bat.html)
