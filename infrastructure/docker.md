# docker run
docker run -dit --name mycontainer -p 8080:80 -v "$PWD":/usr/ httpd:2.4
-d: デタッチモード。バックグラウンド実行
-i,-t: コンテナをキーボードとディスプレイから操作する
-p host:container ; ホストのポートとコンテナのポートを紐づける
-v host:container : ボリュームマウント
--name: コンテナ名指定

停止中のコンテナでシェル実行（シェル終了でコンテナ終了）
docker run --name [container] -it httpd:2.4 /bin/bash
実行中コンテナでシェル実行（シェル終了でもコンテナ稼働のまま）
docker exec -it [container] /bin/bash

デタッチ
CpCq
アタッチ
docker attach [container]

## 1回限り実行するコンテナ（ビルドなど）
docker run --rm -v host:container -w [workdirectory] golang:1.14 go build -v
--rm: 実行完了後コンテナ破棄
-w: 作業ディレクトリ指定（このディレクトリ内でコマンドが実行される

# docker image
* [ls,rm]
* [prune] 一括削除
* [inspect] 詳細表示
* [build] Dockerfileからイメージを作成
* [push,pull] DockerHubへ登録・取得

# docker container
* [run] コンテナの作成と起動
* [logs] コンテナのログを取得
* [start,stop] 起動・停止
* [create,rm]
* [prune,inspect] imageと同様
* [attach] コンテナで起動しているシェルに接続。exitで抜けるとコンテナは停止する。CtrlP,CtrlQと打てば停止せずに抜けられる。
* [exec] コンテナに接続してコマンドを実行。ここでシェルを起動した場合、exitしてもコンテナは停止しない。
* [prots] ループバックからの接続のみを想定する場合、127.0.0.1:8000:8000 のように接続元IPにローカルホストを指定すべき。


# docker volume
* [create,ls,rm,prune]


# docker network
* bridge, host, none の3種類がある。

# Dockerfile
* [Dockerfile] dockerイメージの設計書。さまざまな設定をファイルとして保村することで、いつでも再現性のあるイメージファイルを作成できる。
* WORKDIRは作業ディレクトリの指定
* COPY HostPath ContainerPath で指定ディレクトリをコンテナ内にコピーする。
* RUNはビルド時に実行するコマンド。&&で繋げられる。
* CMDは起動時に実行するコマンド。["command", "option", "option"]
* ENVは環境変数。

# docker-compose.yml
* environment: 環境変数の設定
* depends_on: 

# docker compose
* デタッチモードで起動
dockeer compose up -d

## Dockerネットワーク
ポートホワードしないと外部ホストからコンテナへはアクセスできない。
