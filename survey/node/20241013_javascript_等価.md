# 等価

## 厳密等価 `===`

true

```js
'1' === '1'
null === null
undefined === undefined
// 同じ参照
const obj = {}
obj === obj
```

false

```js
'1' === 1
0 === false
null === undefined
// 別参照
{} === {}
```
