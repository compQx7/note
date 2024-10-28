# SQL
## 基本文

### select
SELECT column, ...
  FROM table [AS new_name], ...
[WHERE condition]
[other command]

### update
UPDATE table
   SET column = value, ...
 WEHRE condition

### delete
DELETE
  FROM table
 WHERE condition

### insert
INSERT INTO table
            (column, ...)
VALUES (value, ...)

## 句

### join
JOINはINNERもOUTERもCROSSジョインから抽出する。ONはクロスの１行ごとにtrue/falseを判定しているだけ。真偽不明だが分かりやすい

### null
0 でも 空文字 でもない。
= や <> で判定してはいけない。SQLでは不明や計算不能を示すUNKNOWNがある。
IS NULL, IS NOT NULL


## 演算子

### like
式 like パターン文字列
% : 任意の０文字以上の文字列
_ : 任意の１文字
ex: WHERE memo LIKE '%１月%'

### between
* BETWEEN a AND b
* 状況により、AND演算子より処理性能が悪くなる

### in, not in
式 in (value, ...)
式 not in (value, ...)
= ではひとつの値しか比較できないが in なら複数と比較できる。

### exists, not exists
* ほとんどのexistsは inner joinに置き換え可能で、パフォーマンスもよくなる。
* ほとんどのnot existsは left joinで結合し、WHERE句でIDが null かどうかを判定することで置き換え可能。パフォーマンスが上がる。
* joinの方がパフォーマンスが良くなる理由は、先に join句内ののサブクエリが実行されるため、件数を絞った上で結合するため。

### not in と not exists
* それぞれ回す対象、インデックスが使われる対象が違う。
* INは先に副問い合わせを実行し、主問い合わせをする。EXISTSは主問い合わせを１行ずつ副問い合わせする。
* 最適化しない状態の単純な計算量でいうと、inを使用した場合はテーブルAの行数×テーブルBの行数で、existsを使用した場合はテーブルAの行数×1のイメージ。
* 主クエリの結果から特定の条件を満たす行を排除するために使用される。結果としてあるテーブルに特定条件に合致しないレコードを取得できる。

### any, all
式 基本演算子 ANY (value, ...)
ANY : 値それぞれと比較していずれかが真なら真
ALL : 値それぞれと比較して全てが真なら真

### NOT, AND, OR
優先順位はnot,and,or。( )で優先度を上げられる。
and,orは条件式同士を。notは右辺を逆転させる。

## 集合演算子
集合演算子は列と型が一致するテーブル同士を結合する。
例えば、データが増えてきて、アーカイブテーブルと最新テーブルで分けた場合などに結合できる。
unionは和集合、except/minusは差集合、intersectは積集合。

### union
SELECT 文１
 UNION (ALL)
SELECT 文２


## 検索結果を加工する

### distict
重複を取り除いて、値の一覧を取り出せる。
SELECT DISTINCT column
  FROM table

### order by
asc,desc
SELECT column FROM table
 ORDER BY {column/1} asc, ...
 
 並び替えで、nullを先や後に持ってきたい場合、インライン式でcace式を使ってnullフラグ列を用意する。Oracleではnulls first ( last) が使える。
 
 条件によってソート対象の列を変えたい場合はorder by句でcase式を使う。

### limit
先頭から何番目まで、先頭から何番目を除いて、を指定できる。
SELECT column FROM table
 ORDER BY column
 LIMIT 3    (先頭から３つ)
 LIMIT 1 OFFSET 3   （先頭３番目からひとつ）

## 集計関数
* SUM()などの集計関数は、対象の複数行に対して１度だけ計算される。集計関数だけを用いると１行だけ返されるが、GROUP BY句と合わせて複数行出すこともできる。
SELECT column, 集計関数
  FROM table
 GROUP BY column
HAVING 集計結果に対する絞り込み条件

## 副問い合わせ
* 単一の値の代わりに、１列(複数行)の代わりに、テーブル形式の代わりに、副問い合わせを利用する。
* 複数行の代わりに副問い合わせを利用するパターンでは、IN(副問い合わせ)、ANY/ALL(副問い合わせ)といった感じ。ちなみに NOT IN と <>ALL 、IN と =ANY はそれぞれ同じ意味。副問い合わせ結果にnullが含まれる可能性がある場合は、IS NOT NULL や COALESCE 使ってnullを取り除く。
* テーブル形式の副問い合わせは、SELECTやINSERTで使える。

## その他

### case
CASE (評価する列や式) WHEN [値1/条件1] THEN [値1/条件1の時に返す値]
                    ...
                    ELSE default_value
END

* CASE文は最終的にENDで値を返すもの、と考えればSELECT句で使われようがWHERE句で使われようが理解できる。
* 真になるWHEN句で打ち切られ、残りのWHEN 句は無視される
* 各分岐が返すデータ型は統一する
* ELSEは明示的に書く。暗黙でヌルになる。
* CASE式がやることは単なるラベルの読み替えだが、GROUP BYや集約関数と使うと効果大。

### ストアドプロシージャ
* 一連の処理をひとつのプログラムにまとめているため、ネットワークの負荷を軽減し速度向上が見込める。

# 集合論で考える結合
## 左集合
LEFT JOIN
左側のテーブルのデータを保持しながら、必要に応じて右側のテーブルのデータを結合する場合に使用

## 補集合
### LEFT JOINバージョン
``` sql
SELECT A.*
FROM tableA A
LEFT JOIN tableB B ON A.key = B.key
WHERE B.key IS NULL;
```

### NOT EXISTSバージョン
``` sql
SELECT *
FROM tableA A
WHERE NOT EXISTS (SELECT 1 FROM tableB B WHERE A.key = B.key);
```

## 全体集合
FULL JOIN
キーなどを条件に、左集合と右集合の結合を合わせたもの。どちらかの集合にしかないものも含む。

## 和集合
UNION (ALL)

## 共通部分
inner join

## 全組合せ
CROSS JOIN


# パフォーマンス
* SQLのパフォーマンスを改善するには、RDBMSがSQL文を内部でどのように処理しているか理解すること。SQL文の解析、SQL文の書き換え、実行計画の作成
* SELECT句の実行順は、FROM (JOIN), WHERE, GROUP BY, HAVING, SELECT, OREDER BY
* カラムは必要なものだけ指定する。（アスタリスクを使わない）
* 句は大文字で記述することでキャッシュを使う確率があがる。
* WHERE句で関数利用や計算をするとインデックスが使われない。
* 結合前に絞る。結合後の大きなテーブルにWHERE句で条件を絞るのはコストが高い。
* テーブル名には別名を付け、カラムはテーブル名から指定する。
* インデックスカラムで結合する。
* order byもインデックスカラムを指定する。
* in句と違い、EXISTS句はひとつでも見つかれば走査終了する。
* バルクインサート
* 並べ替え処理はDBにとって処理の重い仕事。distinctやunionも内部的には並べ替えを行なっていることもある。その辺も意識して。

## SQLチューニング
* データ型の見直し
* インデックスの追加・削除
* SQL見直し
* ストアドプロシージャの削除
* 正規化と非正規化

* いかにインデックスを活用するか
* WHERE,ONで加工しているものがあればindexを使うように修正
* なるべくON句で絞っておく（その後のWHERE句を軽くするため）

# tips
* sqlでは値の部分に計算式も入る。
* LENGTH()など文字列・数字・日付・変換といった各行に対する関数も利用できる。ユーザー定義関数やストアドプロシージャも作成できる。
* [SQL実行の仕組み] SQLの解釈、SQLの最適化、実行計画の作成、実行計画の決定の流れで実行計画を作る。

## 条件によってWHERE句を変える
CASE式は値を返すことを利用して、CASE＝END = 値を利用する。
CASE WHEN 〜 (ここに複数条件) THEN 'TRUE' or 'FALSE' ... END = 'TRUE'



# oracle
0埋め
LPAD(word, 桁数, '0')

置換
REPLACE(targetword, beforeword, afterword)

結合
||

文字列抽出
SUBSTR(tartgetword, 取り出したい文字列の位置（負なら末尾から), (取り出す文字列長))

NULLなら NULLでないなら
col IS NULL
NOT col IS NULL

グループ単位に通し番号を振りたい場合
DENSE_RANK() OVER(ORDER BY [グループ項目名を指定])

グループ内で連番を振りたい場合
ROW_NUMBER() OVER(PARTITION BY [グループ項目名を指定] ORDER BY [連番を振りたい順位を指定])
