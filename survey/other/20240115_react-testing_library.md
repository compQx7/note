# test

## 意識すること

- 本当に検証できているか（最初から通っているテストではないか）

  表示確認のテストであれば、実は最初から表示されてしまっていたのではないか、実はもともと表示されていなかったのではないか。

- テストコード上の複雑なロジック（if や for を使った制御）は避ける

  その処理の動作保証がない。

- 「正しい」という言葉を避け、期待する結果を記述する

- テストタイトルには期待するふるまいを記述する

  失敗した時に何が失敗しているのか分かるように。

## React のテスト

Reactはクラスコンポーネントだろうが関数コンポーネントだろうがHTML要素を生成しているに過ぎません。 (=renderの結果。)
なので、まずは HTML要素が期待通り出力されている か、 タグおよび属性の構成 を検証しないといけません。
ブラウザはHTML要素の構成及び class に従って CSS で表示を制御しています。
特に改修に伴い所謂デグレードを検知するためにもタグおよび属性の構成に対する検証は必須です。

画面操作に伴うイベント等の動作に対しては何をしないといけないでしょうか。
イベントの結果として何が起きるのでしょう？。
Reactコンポーネントとしては結局はHTML要素が出力し直される (=renderが再度実行される) に過ぎません。
イベントの結果として出力し直されないことがあるのか？
その場合は出力されているHTML要素が変わっていないことを検証しないといけません。

画面での入力項目が多ければ多いほどパターンが増えます。
例えば入力項目が10あると、例えON/OFFの二値でもパターンは2の10乗となり全パターンなど書いてられません。
しかしON/OFFの値に従って結果を返すコンポーネントを組み合わせておけば、ON/OFFの二通りをそのコンポーネントでテストして、組み込んだ側は入力値に関係なくON/OFFの結果のコンポーネントが返ってくることだけを検証すればいいですよね。

イベントでの処理内容そのものに対してはどうすればよいでしょうか。
一つの処理で多岐にわたる内容を書いてしまうのもテストパターンが増えてよろしくありません。
なので、そういう処理はコンポーネントとは分離して記述しておき、単独でテスト出来るようにしておきます。
そうすることでテストパターンを減らし、かつテスト自体も容易にします。

## RTL

RTLのrender関数は、任意のJSXを受け取ってレンダリングします。その後、テスト内でReactコンポーネントにアクセスできるようになるでしょう。その確認のため、RTLのdebug関数を利用できます。
コマンドラインでテストを実行すると、AppコンポーネントのHTMLを確認できます。React Testing Libraryでコンポーネントをテストする際は、まず最初にコンポーネントをレンダリングしてから、テスト内のRTLのレンダラーで見えるものをデバッグしていきます。こうすることで、自信を持ってテストを書くことができます。
Reactコンポーネントではなく、このHTML構造が出力されます。

```javascript
describe('App', () => {
  test('renders App component', () => {
    render(<App />);

    screen.debug();
  });
});
```

userEvent でなく @testing-library/react" には fireEvent が含まれています。
しかし、原則として userEvent を使うようにして下さい。
userEvent の方がブラウザの動作に近い形でイベントが発行されます。
例えばブラウザ上であるオブジェクトをclickした際にはclickイベントだけでなくfocusイベントもまた発生します。
fireEvent は指定したイベントのみ発行されますが、 userEvent はそれら関連するイベントも発行されるのでよりブラウザ上の動作に近い形になります。
一方で userEvent は全てのイベントに対応しているわけではないので未対応のものは fireEvent を使います。

### 検索バリエーション

要素が存在しないことをアサーションする場合にはqueryByを使い、それ以外の場合は標準でgetByを使います。
findBy検索バリエーションは最終的に存在する非同期要素に使われます。
複数要素を検索する場合は、getAllBy,queryAllBy,findAllBy

### setState の反映

- setState は即反映されるわけではない。
- act() を使う
  - ただし RTL の render() / fireEvent / userEvent で処理を呼び出す場合は act() を使う必要はない。なぜならば RTL が内部で使用しているから。
  - イベントハンドラや React のライフライクルメソッド( useEffict フックも含む)内で非同期関数を呼び出して、その非同期関数内で setState している場合は別途対応が必要。非同期処理が呼び出されるのが act() を抜けた後に呼び出されるから。
    - この場合は waitFor() で待つ。waitForの処理としてexpect()を記述することにより、それが成功するかタイムアウトする迄待つ。waitForのタイムアウト値は既定で1000ms。

## Jest

### mock

テスト対象

```js
export function forEach(items, callback) {
  for (let index = 0; index < items.length; index++) {
    callback(items[index]);
  }
}
```

テストコード

```js
const forEach = require('./forEach');

const mockCallback = jest.fn(x => 10 + x);

test('forEach mock function', () => {
  forEach([0, 1], mockCallback);

  // モック関数は2回呼び出されること
  expect(mockCallback.mock.calls).toHaveLength(2);

  // 関数の最初の呼び出しの第一引数は0であること
  expect(mockCallback.mock.calls[0][0]).toBe(0);

  // 関数の2回目の呼び出しの最初の引数は1であること
  expect(mockCallback.mock.calls[1][0]).toBe(1);

  // 関数への最初の呼び出しの戻り値は10であること
  expect(mockCallback.mock.results[0].value).toBe(10);
});
```

### beforeEach, afterEach

- describeでネストしていても、テストケースごとに実行される様子。

```js
describe("describe layer 1", () => {
  beforeAll(() => console.log("describe layer 1 > beforeAll"));
  beforeEach(() => console.log("describe layer 1 > beforeEach"));
  afterEach(() => console.log("describe layer 1 > afterEach"));
  afterAll(() => console.log("describe layer 1 > afterAll"));
  test("describe layer 1 > test", () => console.log("describe layer 1 > test"));

  describe("describe layer 2", () => {
    beforeAll(() => console.log("describe layer 2 > beforeAll"));
    beforeEach(() => console.log("describe layer 2 > beforeEach"));
    afterEach(() => console.log("describe layer 2 > afterEach"));
    afterAll(() => console.log("describe layer 2 > afterAll"));
    test("describe layer 2 > test", () => console.log("describe layer 2 > test"));
  });
});
```

```sh
$ jest describe.test.js 
    describe layer 1 > beforeAll
    describe layer 1 > beforeEach
    describe layer 1 > test
    describe layer 1 > afterEach
    describe layer 2 > beforeAll
    describe layer 1 > beforeEach (*1)
    describe layer 2 > beforeEach
    describe layer 2 > test
    describe layer 2 > afterEach
    describe layer 1 > afterEach (*2)
    describe layer 2 > afterAll
    describe layer 1 > afterAll
```

### 例外

次の様にexpect()に対してtoThrow()Matcherで検証出来ます。

```Js
// 何等かの例外が発生するか否か
expect(() => {
  // 例外が発生する同期処理のコードを記述する。
}).toThrow();
// InternalErrorが発生するか否か
expect(() => {
  // 例外が発生する同期処理のコードを記述する。
}).toThrow(InternalError);
```

非同期処理の場合はasync/awaitで記述出来ますが、下記の様にMatcherの前に rejects の指定が必要です。

```js
// 例外が発生するか否か
await expect(async () => {
  // 例外が発生する非同期処理のコードを記述する。
}).rejects.toThrow();
```

### モック

spyOn()とmock()の使い分けですが基本的には下記になります。

- 既存のオブジェクト/メソッドをmock化する場合はspyOn()を使う。
- 新たにオブジェクト/メソッドを生成して利用する場合は mock() を使う。

mockの初期化には下記があります。
mockClear()とmockReset()では置き換えられたままなので注意して下さい。

- mockClear
  呼び出し回数や呼び出し履歴を初期化する。
  実装(.mockReturnValueやmockImplementationで設定した内容)はそのまま。

- mockReset
  呼び出し回数や呼び出し履歴及び実装(.mockReturnValueやmockImplementationで設定した内容)も初期化する。

- mockRestore(spyOnしたmockのみ。replacedPropertyはrestore)
  mockに置き換える前の状態に戻す。
  要するにmockではなくなる。

## memo

screen.getByTestId()
RTL のscreenオブジェクトに用意されたメソッドです。
data-testid属性の値を持つ要素を取得します。

toHaveClass()
jest-dom に用意されたMatcherです。
指定した要素に指定した class が存在するかを検証します。

toHaveAttribute()
jest-dom に用意されたMatcherです。
指定した要素に指定した値を持つ属性が存在するかを検証します。
disabled のように値を持たない場合は属性名だけを渡します。
上の例ではnotを付けているので disabled が設定されていないことを検証しています。

toBeTruthy()
jest に用意されたMatcherです。
