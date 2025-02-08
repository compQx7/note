# powershell

```ps1
# 設定ファイルの場所確認
$PROFILE
```

```ps1
# 設定ファイルを強制的に作成
New-Item -ItemType File -Path $PROFILE -Force
```

```ps1
function MyCommand {
  param($Name)
  Write-Output "Hello $Name"
}
```

```ps1
# プロファイルの再読み込み
. $PROFILE
```
