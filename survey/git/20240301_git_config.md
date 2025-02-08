# git config

設定を確認

```sh
git config [--system|--global|--local] -l
git config user.name
```

個別の config設定を解除する場合

```sh
git config --unset --global user.name
```

WSL の Git の`creadential.helper`をWindowsの Credential Manager を見るようにする。

```sh
git config --global credential.helper "/mnt/c/Users/username/AppData/Local/Programs/Git/mingw64/libexec/git-core/git-credential-wincred.exe"
```

## name and email

```txt
git config [--global] user.name <yourName>
git config [--global] user.email <yourEmail>
```

## global setting .gitignore

共通して除外したい内容を `~/.gitignore` などに定義して、
global 設定に追加する。

```sh
core.excludesfile=~/.gitignore
```

例:

```.gitignore
my-*
```

## トラブルシューティング

### https による git clone が失敗する

古い認証情報でアクセスしている可能性がある。

キャッシュクリアする

```sh
git credential-cache exit
```

手動で接続する

```sh
git clone https://<username>:<token>@gitlab.{repository}.git
```

### SSH 接続している Windows ではパスワードが保存されない

SSH 接続している Windows では GCM （Git Credential Manager）にパスワードが保存されない。

ストアを dpapi に変更してファイル保存にする。

```sh
git config --global credential.credentialStore dpapi
```

[Credential stores](https://github.com/git-ecosystem/git-credential-manager/blob/release/docs/credstores.md)

### GitLab のバージョンアップにより リモートリポジトリにアクセスできなくなった

状況

- ターミナル、VS、VSCode で git fetch などのリモート接続を行うとエラーが発生する。
  fatal: unable to access 'https://gitlab.独自ドメイン/リポジトリ': The requested URL returned error: 500
- ブラウザでは GitLab にログインしてリポジトリを確認できる。

試したこと

- gitlabに対する clone は同様のエラーが発生します。（GitHubの適当なリポジトリはクローンできる）
- Windowsの資格情報マネージャーでgit関連を削除しても解消されず。
- GitLabアカウントのパスワード変更して git fetch 
- Gitの再インストール（設定ファイルも一度削除）
  - git config -l --global を実行しても何もない状態で git fetch

解決

- パスワードが「文字列.数字複数」という構成だったため、ドットを使わない方式に変更したところ、すべての端末でアクセスできるようになった。
- サーバーエラーは`TypeError (no implicit conversion of String into Integer):`
  となっており、もしかすると JWT が HEADER、PAYLOAD、VERIFY SIGNATURE をドットで区切る構成ので、後者2つのどちらかを誤認識させてしまった可能性がある。
- （追記）ドットの前がbase64でデコードして数値になる文字列だと再現する様子。
  - Nm.12345678（NG）
  - Nw.12345678（NG）
  - Aa.12345678（OK）
  - Zi.12345678（OK）

- base64 デコードで数字になるパターン：`[M-N][A-Za-z0-9+/]|O[A-Za-f]`

## 参考サイト

- [アクセストークン](https://zenn.dev/miya789/articles/manager-core-for-two-factor-authentication)
