# Python 環境構築

## 前提

pythonがインストールされており、pythonとpipにパスが通っていること。
※pipはScriptsフォルダにある。

## 構築

仮想環境構築コマンド

```sh
python -m venv <EnvName>
```

仮想環境アタッチ

```sh
# Linux
source ./<EnvName>/Scripts/activate
# Windwos
./<EnvName>/Scripts/activate
```

仮想環境デタッチ

```sh
deactivate
```

パッケージのインストール

```sh
pip install <package>
```

requirements.txt に保存

```sh
pip freeze > requirements.txt
```

requirements.txt を基にパッケージをインストール

```sh
pip install -r requirements.txt
```
