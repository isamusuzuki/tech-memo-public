# Chrome OS 特有のトラブルシューティング

作成日 2021/01/04

どちらもVisual Studio Codeに関係している

## 1. `sudo apt update` で毎回「スキップします」メッセージが出る

vscodeに関係するメッセージが出る

```text
N: Skipping acquire of configured file 'main/binary-arm64/Packages' as repository 'http://packages.microsoft.com/repos/vscode stable InRelease' doesn't support architecture 'arm64'
N: Skipping acquire of configured file 'main/binary-armhf/Packages' as repository 'http://packages.microsoft.com/repos/vscode stable InRelease' doesn't support architecture 'armhf'
```

解決方法

```bash
sudo vi /etc/apt/sources.list.d/vscode.list

# deb [arch=amd64,arm64,armhf] http://packages.microsoft.com/repos/vscode stable main
# => deb [arch=amd64] http://packages.microsoft.com/repos/vscode stable main
```

参考にした記事

[linux \- VS Code N: Skipping acquire of configured file 'main/binary\-arm64/Packages' \- Stack Overflow](https://stackoverflow.com/questions/65306968/vs-code-n-skipping-acquire-of-configured-file-main-binary-arm64-packages)

## 2. 設定の同期がうまくいかない

以下のエラーメッセージが表示されて、失敗する

```text
Error Message: The name org.freedesktop.secrets was not provided by any .service files
```

解決方法

```bash
sudo apt install gnome-keyring -y
```

参考にした記事

[The name org\.freedesktop\.secrets was not provided by any \.service files · Issue \#1515 · microsoft/vscode\-docker](https://github.com/microsoft/vscode-docker/issues/1515)
