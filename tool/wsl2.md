# wsl
- wsl自体はひとつのVM、各ディストロはコンテナ。
エクスプローラーでwslと入力すれば、その階層をwslで開ける。

Windows11以降なら
```powershell
wsl --install
```

## WSL2をエクスプローラーで開く
エクスプローラーで`\\wsl.localhost\[vm]\`

もしくは

```powershell
[/mnt/c/windows/]explorer.exe [path]
```

# 起動
wsl -d <[DestributionName]> [-u UserName]

# 設定ファイル
各ディストリビューションの`/etc/wsl.conf`
```ini
[user]
default = testuser
```

# 同じディストリビューションの複製
tarファイルを経由して複製する方法
```powershell
wsl -l
wsl export <[DestroName]> [ExportFilePath]
wsl import <[DistroName]> <[InstallLocation]> <[ImportFile]>
```
InstallLocation: VHDファイルを保存するディレクトリ名

標準入出力を利用して複製する方法
```powershell
wsl --export <[FromDistroName]> - | wsl --import <[ToDistroName]> <InstallLocation> -
```

# リセット
Windowsの設定、アプリ > アプリと機能 > Ubuntu > 詳細オプション からリセットする。