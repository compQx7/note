# git worktree

## 基本

ひとつのgit管理で複数のブランチの作業を別ディレクトリ（ワークツリー）で行うことができる。

`git branch`で見ると Worktree は`+`で表示される。

Worktree でチェックアウトしているブランチはチェックアウトできない。

## 使い方

git管理のリポジトリで Worktree を追加する。

```sh
git worktree add <path> <branch>
git worktree add ../branch1 feature/bug1
```

以下のような構成になり、`branch1`ディレクトリに移動することで、`feature/bug1`ブランチとして作業できる。

ワークツリー作成前
project
`── mainディレクトリ/
    |── src/
    |── test/
    |── README.md
    `── ...

ワークツリー作成後
project
|── mainディレクトリ/
│   |── src/
│   |── test/
│   |── README.md
│   `── ...
`── branch1ブランチ用ディレクトリ/
    |── src/
    |── test/
    |── README.md
    `── ...

```sh
# Worktreeの削除
git worktree remove <path>
# Worktree一覧を表示
git worktree list
```
