---
tags: python
---

# pipenv

作成日 2019/08/07

## pipenvとは

venvのさらに一歩先を行く、Node.jsのようなプロジェクト管理ツール

```bash=
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
