# 基本
* AOP、DI、JDBCMVC、Test、TX
* [AOP] アスペクト指向プログラミング。例外処理やロギング、トランザクションなど、クラスを横断した処理をビジネスロジックから分離する。
* DIは変数にインスタンスを代入すること。
* @componentなどを付与することで、DIコンテナという入れ物にBeanとして登録される。BeanとはSpringがDI対象として管理するクラスのこと。

* application.propertyにDB接続情報を記載。
* Controllerがリクエストを受け取り、モデルやRepositoryに処理させ、返ってきたものをControllerがビュー（タイムリーフ）に伝えてレスポンス。

* [@AllArgsConstructor] Lombokのアノテーション。全フィールドへ値をセットするコンストラクタ。
* [@Data] Lombokのアノテーション。getter,setterを自動実装してくれる。

## MVCのフレームワーク
* [MVCを使う場合のヒント] モデルにできることはすべてモデルにさせる。ビューの外観について知らなければできない仕事はモデルにさせない。外観とビジネスロジックを分離するのがMVCの役割。ビューとモデルを可能な限り分離して、残った部分がコントローラー。同じ処理をwebアプリだけでなく、コンソールやWindowsフォームでも実装したいと考えた時、ビューは3種類になるが、モデルはひとつを使い回せる。
* [MVCに拘らない開発] MVCという3つのオブジェクトのみで開発するとなるとどうしても肥大化したオブジェクトが生まれる。それならばMVCに拘らず、本来のオブジェクト指向で考えて、レイヤー化アーキテクチャ、ドメイン駆動、MVCなどを適切に使い開発する。

## Thymeleaf
* [postエラー] postで何故かnullしか渡せない時。おそらくinputタグのth:fieldが鍵と思われる。postしたくてもうまくいかない時は、表示するだけの値を送りたい場合は、inputタグのタイプ要素でhiddenを使ったり、th:eachの中の場合は、field要素で[__${stat.index}__].propertyを使うとうまくいく。
* [thymeleaf] テンプレートエンジンのひとつ。HTMLに文字埋め込んだり読み込んだりできる。
* [th:each] th:each="name:${names}" で、addObjectで渡されたList型のnamesオブジェクトをひとつずつnameとして取り出す。Entityインスタンスの数だけtagが生成される。それを子tagで th:text="${name.property}" で呼び出す。
* [th:href] th:href="@{path}" cssを指定するlinkタグにも使える。th:href="@{/css/file}"。URLリンク式 @{} の中で __${object.property}__ とするとプロパティ値を埋め込むことができ、aタグと合わせれば、押された情報を送信できる。その場合、Controllerのhandlerメソッドでは"/~/{property}"をマッピングし、取得する情報を引数にとり、@PathVariable(name="property")アノテーションをつける。
* [th:object] th:object="${name}" でそれよりも下位要素で使用するオブジェクトをnameとして指定。このオブジェクトからは *{upropertiy}でプロパティを取得できる。object.GetPropertyの結果が取得できる。th:valueなどで使われる。
* [#アクセス] ${#class.-} でclassにアクセスしてメソッドなどが使える。
* [th:if] タグの表示非表示をコントロールする。divタグで"${#fields.hasErrors("property")}" th:errors="*{property}" th:errorclass="classname" でpropertyに関するエラーの有無を返す。th:errorsはエラ〜メッセージを取得する。th:each="e:${#fields.detailedErrors()}" th:text="${e.message}" で一箇所にまとめてエラ〜表示できる。
* [th:field] htmlのid,name,value属性を生成。別で指定することもできる。optionはid,nameは持たない。fieldには、value属性の値と*{property}の値が同じなら、checked属性(optionならselected属性)を追加する働きがある。
* [radio,option] 同じ名前の要素を複数用意した場合（選択式のUI）、idは連番が追加される。
* [checkbox] 未チェック状態もフォームにバインドできるよう、<input type="hidden" name="!対応するcheckboxのname" value=  >を用意する。
* [form内に複数ボタンを設置] inputタグではなく、buttonタグでtype="submit" th:formaction="@{request_url}" とする。
* [th:switch,th:case]

## Controller
* [@Controller] Thymeleafが処理したHTMLを返す。
* [@GetMapping,@PostMapping,@RequestMapping] メソッドにつける。ブラウザからのリクエストを受け取り処理するためのアノテーション。
* [@RequestParam] 引数につける。リクエストパラメータを引数に取った処理を書ける。
* [ModelAndViewオブジェクト] 引数にする。モデルはビューで使用するデータ、ビューは次に表示する画面名を指す。setViewNameでHTMLファイル名を渡す。addObject("Thymeleafに対応させる名前", メソッド内オブジェクト（変数）名:)でビューが使うデータを渡す。return で返す。まっさらなFormを渡すときは引数の中でnewする。もしview名を保持する必要がない場合は、Modelで置き換え、String型でformを返す。
* [@ModelAttribute] 引数につける。多くのパラメータを受け取るとき、@RequestParamで受けると大変なため、Formというクラスのプロパティとして受け取る。Formクラスには@Dataをつけるとformインスタンスのgetterが容易に使えて便利。ModelAttributeが付与されたオブジェクトはaddObject()しなくても遷移先で使用できるため、addObjectは省略できる。ただし名前規則に制限あり。
* [Controller内でRepositoryを使う] private final でRepositoryプロパティを宣言し、@Autowiredをプロパティにつける（フィールドインジェクション）か、@AllArgsControllerをクラスにつける（推奨されるコンストラクタインジェクション）。Repositoryインスタンスのメソッドでクエリ実行。
* [@Autowired] Sprinbootにインスタンスを生成してもらう。プロパティやコンストラクタにつける。コンストラクタがひとつの場合は省略できる。
* [@Validated] @ModelAttributeと一緒に引数につける。BindingResult型(下記参照）引数も一緒に引数に入れる。Formクラスのアノテーションでつけた入力チェックをさせる。このアノテーションでチェックできないエラーはServiceで調べる。（@Serviceを付したクラスを用意し、Controllerでフィールドインジェクション等して利用）、
* [BindingResult引数] @ModelAttributeを付した引数とともに引数にする。これにより、チェック結果がBindingResultに格納される。BindingResut # hasErrors()でbooleanを取得できる。BindingResult # getObjectName()でFormクラス名を取得できる。BindingResult # addError(FieldError型)でエラー情報を付加できる（下記参照）。BindingResultはFormに関連づけられているため、addObject不要。
* [FieldError型] new FieldError(Formクラス（BindingResult型.getObjectName()）, field名, エラーメッセージ)
* [リダイレクト] return "redirect:[path]"

## Form
* [Formクラス] 要はパラメータを受け渡しするためのただのオブジェクトクラス。@ModelAttributeで引数に渡されて意味をなす。フォーム部品をFormプロパティにバインドする。前者にあって後者にない場合は何もバインドされないだけだが、前者になくて後者にあると実行時エラーになる。
* [@NotBlank] 必須入力
* [NotNull] Nullでない。
* [@Min,@Max] 指定値より大きい、指定値より小さい。
* [@Length,@Range] 指定文字数内、指定数値内。
* [@Pattern] 正規表現にマッチする
* [メソッド] フォーム入力データからEntityを生成して返すメソッドを作る。

## Entity,Repository
* [@Entity,@Table,@Data] TableではnameにDBの対応づけるテーブル名を指定。
* [@Id,@Column] プロパティにつける。Idは主キーであることを表す。Columnは対応づける列名を指定。同名なら省略可能。
* [@Repository] JpaRepository<Entity, PKtype>を継承したインターフェースにつける。基本的なCRUD操作メソッドはSprinBootにより自動実装される。そのほか命名規則に準じた抽象メソッドを定義するとそれも自動実装される。
* [findById] 主キーを元に検索する自動実装メソッド。Optional型を返すため、get()メソッドで値を取得する必要がある。主キーのため、結果は１件かnull。

* [spring handler] Controllerクラスでパブリックで定義されたメソッド。アノテーション次第でいろんな値を返せる。テンプレートに反映させたり、他ページへリダイレクトさせたり、ファイル書き出したり。

# Spring Batch
* ジョブが複数のステップを扱う
* ステップに入れる処理のモデルとしてには、read, process, writeを実装するチャンクモデル、自由にロジックを入れるタスクレットもでるがある。
* チャンクモデルの中間コミットは指定した回数だけreadとprocessが呼ばれ、writeが一回実行される。
* チャンクモデルは1レコードずつ read, writeなどが呼ばれる。プロセッサーは複数連結可能。また、例えばプロセッサーでヌルを返せばそのレコードはパスされる。
* トランザクションのコントロールやジョブ渡し引数は可能。
* JobRepositoryはバッチの実行結果などをDBに保存してくれる機能。
* ジョブやステップ（タスクレットとチャンク）のクラスは複数実装する時には、@StepScopeをつける（シングルトンではなくなる）。また@ComponentをつけてBeanに登録する。
* バッチ設定クラスには@Configurationをつける。これによりクラス内で@Beanを付けることで登録できる。@EnableBatchProcessingを付けるとJobBuilderFactory、StepBuilderFactoryにDIできるようになる。
* なるべく1つのプロジェクトに1つのバッチのみ定義する。
* 主にログ出力で Listenerを使う。ジョブやステップの前後などに実行できる。
* JobExecutionContext、StepExecutionContextで変数を渡せる。
* バッチでパラメータを受け取る場合はタスクレットクラス内のフィールドに@Value()で取得可能

# JSP
* [ Javaの埋め込み] <%   %>で囲まれた部分をJavaプログラムとして読み込む。
* [値の受け取り] request.getParameter()
* [response] JSPからブラウザに対してresponseを返すことでブラウザに動作を指示できる。redirectなど

# サーブレット
* [サーブレットとは] JSPと同様、ブラウザからのリクエストを受けてレスポンスを返すJavaプログラム。役割はJSPが表示機能、サーブレットが処理機能と分ける。サーブレットを継承したクラスを作成し、xmlファイルに登録する。
* [処理の流れ] サーバーが/demoを受信、どのサーブレットクラスを実行するかweb.xmlを確認、/demoというurl-patternを見つけ、それに紐づいているクラスをservlet-nameで探して、実際のサーブレットクラスを実行。GETやPOSTメソッドに応じてサーブレットクラス内のdoGetなどを実行し分ける。

# その他
* DBアクセスなら、Controller→サービスクラス→Repositoryの流れ(springフレームワーク)。RepositoryはEntityをList型で返す。


# spring

## SpringBoot
application.propertiesファイルはSpringBootプロジェクトの環境設定ファイル。

## MVC
* デザインパターン「フロントコントローラーパターン」
* すべてのリクエストを受信するフロントコントローラーであるDispatcherServletがリクエストを受け取り、コントローラーのリクエストハンドラメソッドを呼び出す。コントローラーはサービス処理を実行し、処理結果をモデルに設定して、ビュー名を返す。それを受け、DispatcherServletがビュー名に対応するビューに対して画面表示処理を依頼をする。
* コントローラーには@Controllerを付与。リクエストのハンドラメソッドをビュー名にすることで、テンプレートエンジンのビューがレスポンスのHTMLを生成する。
* @RequestMappingはリクエストハンドラメソッドとURLをマッピングする。
* モデルはビジネスロジック（提供するサービスの処理）、ビューは入出力の表示、コントローラーはモデルとビューの制御を担う。
* MVCお互いに依存しない設計をするのがポイント。
* ModelクラスはControllerからViewへ変数を渡すためのもの。SpringMVCが提供するModelインターフェイス（MVCのモデルでは厳密には違う？）は、処理したデータをビューで表示したい場合にデータを引き渡す（addAttribute）役割を持つ。またオブジェクトを格納し管理する機能がある。リクエストハンドラメソッドの引数に渡せば自動的にインスタンスを設定してくれる。
* ModelクラスはControllerメソッドを制御しているSpringMVCの制御層が自動的に作成したui.Modelのインスタンス。@RequestMappingや@GetMappingなどのControllerメソッドの引数に指定することで、SpringMVCから自動的に渡される。
* Model、ModelAndView、BindingResultなどは、引数に指定された時にSpringMVCが自動的にインスタンスを格納するもの。
* ビューは resources/templates フォルダにビュー（HTML）を配置する。

## アノテーション
外部ソフトウェアにやってほしいことを伝える。
インスタンス生成アノテーションは@Controller（アプリケーション層）、@Service（ドメイン層）、@Repository（インフラストラクチャ層）、@Component（その他）
@ModeAttribute
Controllerのメソッドか引数に付けて利用する。モデルクラスとバインドする。
    @RequestParamの記述が省略できる
    model.addAttributeの記述を省略できる
引数につける場合、BindingResult型を次の引数につけることでバインドの成功可否を受け取れる。

## DI
依存する部分を外から注入する。「使われる側」のクラスを変更する場合、「使う側」のクラス内でも変更が生じる。これを依存性があるという。
DIコンテナにインスタンス生成を任せるために、「インターフェイスを利用して依存関係を作る」「インスタンスを明示的に生成しない」「クラスにアノテーションを付与する」「インスタンスを利用したい箇所にアノテーションを付与する」「SpringFrameworkにインスタンスを生成させる（アプリ起動時のコンポーネントスキャン）」。
3種類ある。コンストラクタインジェクション（推奨）、セッターインジェクション、フィールドインジェクション（非推奨）
コンストラクタインジェクションはフィールドはfinalにできるうえ、テスト時にDI対象をモックオブジェクトに変更しやすい。
コンストラクタがひとつのクラスの場合は、@Autowiredを省略可能。
コンストラクタインジェクションにすることで依存関係が分かりやすい。クラスに多く責任を持たせているかも分かりやすい。
フィールドインジェクションではフィールドをfinalで不変にできない。

## AOP
AOPとはプログラムの中心的関心事と横断的関心事を分離して考えること。ログ出力やトランザクション管理はSpringの機能に任せてプログラムに集中できる。

## Entity
テーブルの１レコードとの対応付けオブジェクト
登録・更新時にDBへ値を渡すため、DBから値を受け取るため

## Repository
データベースへのデータ操作を担当
インターフェイスを定義したうえで実装する。repositoryを利用するクラスでは、インターフェイスのフィールドに実装クラスをDIする。
どんなメソッドがあるか調べるといい。
saveは@Idが付与されたフィールドがnullの場合はinsert、nullでなければupdate。
CrudRepositoryを継承したインターフェイスには@Repository（インターフェイス生成アノテーション）を付けない。ちなみに実装クラスはSpringDataJDBCが自動実装する。

## リクエストパラメータ
受け取れるパラメータには、ビューでの入力値や隠し設定値はHTTPメソッドのパラメータ、同一ビュー内の複数ボタンの識別値としてボタンのname属性、リンクのURLの一部がある。
パラメータの取得方法は、@RequestParamを利用するか、Formクラス（POJO）で受け取る。その他@PathValiableやリクエストマッピングアノテーションのparams属性で取得する方法もある。

# O/Rマッパー
オブジェクトとRDBをマッピングするライブラリ（クラス）。
CRUDメソッドを使える。

# gradle
必要なライブラリをリポジトリからダウンロードしてくれる。
コンパイルする。
テスト実行、レポート出力。
クラスファイルのアーカイブ。

# JDBCとJPA
JDBCはDBアクセス標準、JPAはORマッパーの標準。

# lombok
@Data
getter, setterを実装。
@NoArgConstructor
デフォルトコンストラクタを自動生成。
@AllArgConstructor
全フィールドの初期化値を引数に取るコンストラクタを自動生成。


# thymeleaf
[[ ]]
タグ内でのインライン処理

th:object
formにバインドするオブジェクトを指定

th:field
input要素のid属性、name属性、value属性をHTMLに出力する。


## Springアーキテクチャ
* Springでの viewはHTMLだけでなく、JSONやCSVなどのさまざまなレスポンスを指す。
* サービス層は状態を持たない。アプリケーション層どドメイン層に配置されるサービスはそれぞれ役割が異なる。
* アプリケーションサービスはユースケースを表現するためのサービス。アプリケーションとして提供する機能を実装する。
* ドメインサービスは、ドメインオブジェクトであるエンティティや値オブジェクトを客観的に評価したり、利用したりする機能を実装する。
* リポジトリはデータアクセスを抽象化し、サービスから中の実装を隠蔽する。インターフェイスはコレクションを操作するような形で定義する。インターフェイスはドメイン層、実装はインフラストラクチャー層で定義。
* DispacherServletはすべてのリクエストのエントリーポイント。様々な役割のインターフェイスを呼び出して、フレームワーク全体を制御。中にコンテナを持ち、その中でBeanを管理している。
* HandleappingはパスやHttpメソッドを元に呼び出すHandlerを特定する役割のインターフェイス。
* HandlerAdapterはHandlerを呼び出す役割のインターフェイス。呼び出し前後に適宜処理を行う（バリデーションやJSON変換）
* handlerはリクエストに対する具体的な処理を行う。Viewを使用する場合は、その名前を返し、必要なデータはモデルに詰めて返す。
* ModelはHandlerからViewにデータを受け渡すために利用されるインターフェイス。
* ViewResolverはView名を元にViewの実装クラスを特定し、Viewの完全パスを返す。
* Viewはレスポンスデータの生成を行う役割。
