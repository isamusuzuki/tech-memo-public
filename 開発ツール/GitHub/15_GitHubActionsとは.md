# GitHub Actionsとは

作成日 2025/10/20

## 1. 解説記事を読む

[GitHub Actionsって何？触ってみて理解しよう！入門・逆引きリファレンス #GitHubActions - Qiita](https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e)

- 自分のリポジトリのActionsタブを開く。はじめての場合は、New Workflowの画面になる
- `setup a workflow yourself`を選択すると、yamlファイルを編集する画面になる
- yamlファイルは自分のリポジトリにコミットされる => `.github/workflows/{name}.yaml`

### ワークフローについての情報を定義するパート

```yaml
name: CI  # ワークフローの名前を定義
on:       # どのタイミングでワークフローが走るか
    push: # mainブランチにコミットがpushされた時
        branches: [ "main" ]
    pull_request:      # mainブランチにプルリクが作成された時
        branches: [ "main" ]
    workflow_dispatch: # 明示的にボタンを押した時
```

### ワークフローの中身を定義するパート

```yaml
jobs:
    build:
        runs-on: ubuntu-latest
    steps:
        - uses: actions/checkout@v3   # リポジトリのデータを読み込む
        - name: Run a one-line script
          run: echo Hello, world!
        - name: Run a multi-line-script
          run: |
            echo Add other actions to build,
            echo teest, and deloloy you project.
```

## 2. 公式ガイドを読む

[GitHub Actions ドキュメント - GitHub Docs](https://docs.github.com/ja/actions)

[GitHub Actions のクイックスタート - GitHub Docs](https://docs.github.com/ja/actions/get-started/quickstart)

[GitHub Actions のチュートリアル - GitHub Docs](https://docs.github.com/ja/actions/tutorials)

[サンプル ワークフローの作成 - GitHub Docs](https://docs.github.com/ja/actions/tutorials/create-an-example-workflow)
