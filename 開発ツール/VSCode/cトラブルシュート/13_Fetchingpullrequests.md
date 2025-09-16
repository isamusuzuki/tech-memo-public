# "Fetching pull requests ..." 通知が出る

作成日 2025/09/16

## 1. 通知の内容

```text
Fetching pull requests for remote origin with query failed.
please check if the repo undefined is valid.

ソース: GitHub pull request
```

GitHub Pull Requestsという機能拡張が出すエラーであることがわかった

[GitHub Pull Requests - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)

[microsoft/vscode-pull-request-github: GitHub Pull Requests for Visual Studio Code](https://github.com/microsoft/vscode-pull-request-github)

## 2. 問題を解決する

[Fetching pull requests for remote 'origin' failed, · Issue #1941 · microsoft/vscode-pull-request-github](https://github.com/microsoft/vscode-pull-request-github/issues/1941)

1. 機能拡張枠の中で、この機能拡張を右クリックする
2. 「アカウント設定」をクリックする
3. 適切なアカウントを選ぶ（もしくは新しいアカウントを追加する）
4. VSCodeを再起動する
