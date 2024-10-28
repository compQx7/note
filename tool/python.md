
# 仮想環境作成
python -m venv [EnvName]
EnvNameは.venvが一般的？

# 仮想環境アクティベート
Linux
source [EnvName]/bin/activate
. [EnvName]/bin/activate
Windows
.\[EnvName]\Scripts\activate

VScodeでvenv環境を使う場合は、デフォルトで指定されているパス配下のほか、ワークディレクトリ配下のvenv環境も認識される。

# 仮想環境ディアクティベート
deactivate

# packageインストール
pip install [PackageName]

# packageの確認
pip freeze
