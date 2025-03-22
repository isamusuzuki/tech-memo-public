# venv の次のプロジェクト管理ツール

作成日 2019/08/07

## 01. pipenv とは

venv のさらに一歩先を行く、Node.js のようなプロジェクト管理ツール

```bash
# インストール
pip install pipenv

# プロジェクト初期化
pipenv --python 3.7.4
#=> Pipfileができる

# パッケージのインストール
pipenv install numpy
#=> Pipfile.lockが自動生成される

# 開発用パッケージのインストール
pipenv install --dev autopep8 flake8

# 環境の再現
pipenv install
pipenv install --dev

# 登録したスクリプトの実行
pipenv run start

# 仮想環境に入る
pipenv shell

pipenv --venv

pipenv --rm
```

## 02. Poetry とは

`pyproject.toml`という設定ファイルでパッケージその他を管理するツール

公式トップ => [Poetry \- Python dependency management and packaging made easy\.](https://python-poetry.org/)

紹介記事 => [2020 年の Python パッケージ管理ベストプラクティス \- Qiita](https://qiita.com/sk217/items/43c994640f4843a18dbe?utm_campaign=popular_items&utm_medium=feed&utm_source=popular_items)

いきなり、TOML ファイルで管理と言われてもちょっと戸惑うかも
