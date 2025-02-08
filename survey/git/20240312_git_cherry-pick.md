# git cherry-pick

```sh
# 指定コミットの変更内容を取得・コミットする
git cherry-pick <commit>
# 指定コミットの変更内容を取得する。
git cherry-pick -n <commit>
# 指定ブランチの先頭コミットの変更を取得
git cherry-pick -n <branch>
# 指定ブランチとの差分変更を取得
git cherry-pick -n ..<branch>
git cherry-pick -n ^HEAD <branch>
```

```sh
# cherry-pick した直前コミットを取り消す
git reset [--soft|--hard] HEAD^
```
