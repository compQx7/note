# systemctlとは

## 何をするコマンドか

- initシステムを制御する中央管理ツールのひとつ
- Systemdは、多くのLinuxディストリビューションの 新しい標準となったinitシステム兼システムマネージャー
- 端末でエラーbash: systemctl is not installedが出力される場合、マシンに別のinitシステムがインストールされている可能性があります。
- systemdで、ほとんどのアクションのターゲットである「ユニット」は、systemdが管理方法を把握しているリソースです。

## initシステムとは

- initシステムの基本目的は、Linuxカーネルのブート後に起動させるコンポーネント（従来「ユーザーランド」コンポーネントとして知られる）の初期化です。initシステムは、システム実行中のあらゆる時点でサーバーのサービスとデーモンの管理にも使用されます。

## Unitとは

- ユニットは、それらが表すリソースのタイプによって分類され、ユニットファイルと呼ばれるファイルで定義される。

Unitタイプ（ファイルの拡張子）

| Unit | 説明 |
| --- | --- |
| service | サービス(デーモン) |
| target | ターゲット(複数のUnitをグループ化したもの) |
| mount | マウントポイント |
| swap | スワップ領域 |
| device | デバイス |

## 覚えておくべき使い方とオプション

```sh
# サービスの状態確認
sudo systemctl status [service]
# systemdサービスを起動し、サービスのユニットファイル内の命令を実行
sudo systemctl start [service]
# 停止
sudo systemctl stop [service]
# 再起動
sudo systemctl restart [service]
# 設定ファイルのリロード
sudo systemctl reload [service]
# サービスの自動起動 有効化/無効か
sudo systemctl [enable|disable] [service]
```

```sh
# systemdが解析してメモリにロードしようとしたユニットのみを表示
sudo systemctl list-units
# systemdがロードしないものを含め、systemdパス内の利用可能なユニットファイルをすべて表示
sudo systemctl list-unit-files
```

## 参考

[Systemctlサービスを使用してSystemdサービスとユニットを管理する方法](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units-ja)
