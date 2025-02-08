# 表示・非表示が頻繁なコンポーネントの再レンダリング負荷を軽減
React内部のDOMツリーの構造をJSXの&&演算子でDOMやコンポーネントを出し分けするよりも
代わりにdisplay:noneでトグルしたほうがCSS属性を変えるだけなのでDOMツリーをJSで再生成するよりも安価なため描画パフォーマンスがあがります。
[Using CSS to speed up your React apps](https://itnext.io/using-css-to-speed-up-your-react-apps-a26470829472)

レンダリング頻度が多かったり、レンダリング粒度が大きいuseEffectなどのロジックを持たないコンポーネント（に分離し）使う場合に有効です。

# 参考サイト
- [qiita お前らのReactは遅い](https://qiita.com/teradonburi/items/5b8f79d26e1b319ac44f)
- [React profilerの使い方](https://zenn.dev/virginia_blog/articles/8d202045d5e60f)
- [Chrome Devtools による フロントエンドパフォーマンスの計測](https://zenn.dev/koki_tech/articles/9deb70d0a9befb)

# テクニック
SPAをSSG化

# コード
- PureComponentのshouldComponentUpdate （クラスコンポーネント）
- useCallback, useMemo, memo
  - React.memoでコンポーネント自体をメモ化し、再レンダリング防止
  - 子コンポーネントにコールバック関数を渡す場合は、useCallbackでメモ化し、子コンポーネントの再レンダリングを防止
  - コンポーネント内の計算はuseMemoでメモ化し、再レンダリングが発生しても再計算は防止
- レンダリングに関係のない状態はuseRefで再レンダリング防止
- key制御

浅い比較
