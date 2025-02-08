# git clone, git fetch

## 巨大なリポジトリから取得する

```sh
git clone --depth 1 http://example.com/fuga.git
git fetch --depth 10
git fetch --depth 100 # 値は調整しつつ
git fetch --unshallow # 最後に全部落とす このオプションは最新のgitでないとない
```

下記のようなエラーが出たときは少しずつ取得すると良い

> error failed to get "git://github.com/facebook/react.git": C:\Users\u1166648\AppData\Local\Programs\Git\cmd\git.exe: exit status 128
