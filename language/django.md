# ユーティリティコマンド
django-admin startproject [projectname]
python3 manage.py startapp [appname]

python3 manage.py migrate
python3 manage.py runserver 0.0.0.0:8000


* [makemigrations] models.pyに基づいて、DBの設計図のようなファイルを作成する。都度ファイルを作成する。DBの変更の記録とも言える。
* [migrate] マイグレーション（DBテーブルの作成、変更、削除）を行う。

# django
* [コンポーネント] ルーティング、ビュー、フォーム、モデル、テンプレート。
* [ファイル構成] プロジェクト全体に関わるディレクトリとアプリごとのディレクトリ
* URLは内部でどういった処理をするのか（どのviewを呼び出すのか）を定義するために用いられており、URLとファイルは一対一にはならない。

# setting.py
* [TEMPLATES/DIRS] htmlファイルの格納パスを記述。
* [INSTALLED_APP] 作成したappを認識させる。

# urls.py
path('url', {class.method()|function})
* class-based view と function_based view がある。

# view
* 必要に応じて、フォーム、モデル、テンプレートとやり取りする。画面遷移先の判断をして、ルーティングに処理を委譲する。
* 自身で全ての処理を書くfunctionベースビューとdjango標準機能を継承して使えるclassベースビューがある。
* クラスベースビューがよく使われるらしく、よく使われるクラスやオーバーライドするクラス変数とクラスメソッドはドキュメントを読んでおく。

# form
* django.forms.Formクラスかdjango.forms.ModelFormクラスを継承して作成。
* fieldやvalidationについてもドキュメント必須。

# model
* フィールド、リレーション、モデルオブジェクト

# template
* {% %}
* {{ }}

# wsgi
* アプリケーションサーバー。Webサーバーとdjangoの間でオブジェクトの受け渡しをする。requestをオブジェクトに変換してdjangoに渡し、djangoから受け取ったオブジェクトをresponseとしてWebサーバーに返す。

# admin.py
* ここでmodelsを登録することで、管理画面でテーブルを見ることができる。

# others
* プロジェクト名を変える場合は、manage.py, settings.py, wsgi.py を変え、ディレクトリ名を変える。
