# Web上のGitマージ

## Create a merge commit （未検証）

```sh
git merge --no-ff <branch>
```

何をマージしたか記録が残る（マージコミット）。
マージ前のブランチがそのまま残るので変更を細かく追いやすい
Github上のNetworkやgit log --graphで見たときに複雑になるため注意。
git resetで戻るときに注意が必要。

## Rebase and merge

```sh
# ベースとなるブランチを最新化し、作業ブランチにチェックアウトしてから実行
git rebase <ベースとなるブランチ>
# コンフリクトが発生し、解消したら
git rebase --continue
# コンフリクト解消も完了し、プッシュする時は
git push --force-with-lease
```

```sh
# 操作に失敗したら
git reset --hard <リモートの作業ブランチ>
```

リモートの履歴を書き換えてしまうので、他の人も使っているブランチの場合は注意。

```sh
# リモートブランチの変更をローカルに統合する
git reset --hard origin/<branch>
# あるいは既にローカル変更がある場合
git rebase origin/<branch>
```

リベース前に戻す

```sh
git rebase --abort
```

current はベースとなるブランチの変更で、incoming が作業ブランチの変更。

履歴を1本化できる。

PR の一部を本体に反映させたい場合

```sh
# ベースとなるブランチを最新化し、作業ブランチにチェックアウトしてから実行
# このコマンドでインタラクティブリベース画面になるので、不要なコミットについて pick を drop に変えるか削除する。
# 保存すると pick したコミットだけがリベースされる
git rebase -i origin/develop
```

> nano ctrl + o enter ctrl + x

## Squash and merge （未検証）

```sh
# feature ブランチを develop ブランチにマージしたい時
git switch develop
git merge --squash feature
git commit -m "message"
git push origin
```

```sh
# 作業中に他の人がプッシュしてしまっていてリカバリーする例
git reset --soft <戻りたいコミット>
git stash -u
git fetach --all
git pull
git stash pop
```

作業コミットなどをひとつのコミットにまとめられる。
まとめる前のコミットには戻れないため注意。
