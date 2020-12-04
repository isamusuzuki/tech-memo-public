# Windows ターミナル

作成日 2020/12/04

## Windows ターミナルとは

Microsoft ストアで配布されている新しいターミナルアプリ

ひとつのウィンドウの中に、複数のタブを用意して、コマンド プロンプト、PowerShell、WSLなどを、同時に実行可能

公式リポジトリ => [microsoft/terminal: The new Windows Terminal and the original Windows console host, all in the same place\!](https://github.com/microsoft/terminal)

## Windows ターミナルを使うメリット

- `Ctrl + C`, `Ctrl + V` でコピペができるところ

## Windows ターミナルのデフォルトをWSLに変更する

- Windows ターミナルを起動する
- タブの右隣にある「＋」アイコンのさらに右ある「下向き三角形」アイコンをクリックする
- ドロップダウンメニューの中から「設定」をクリックする
- jsonファイルを開こうとする。デフォルトアプリを VSCode に設定する
- VSCの中で、`settings.json` ファイルを編集する
- `profiles.list` の中にあるWslのguidで、`defaultProfile` の値に上書きするする

```json
{
    "defaultProfile": "{abcdefg-hijklmn-opqrstu-vwxyz}",
    "profiles": {
        "list": [
            {
                "guid": "{abcdefg-hijklmn-opqrstu-vwxyz}",
                "hidden": false,
                "name": "Ubuntu",
                "source": "Windows.Terminal.Wsl"
            }
        ]
    }
}
```

## WSLを起動したときに、WSL側のホームフォルダを開く

- `profiles.list` の中にあるWsl設定に、`startingDirectory`を追加する
- そこに書くディレクトリは、以下を参照する

```json
{
    "profiles": {
        "list": [
            {
                "guid": "{abcdefg-hijklmn-opqrstu-vwxyz}",
                "hidden": false,
                "name": "Ubuntu",
                "source": "Windows.Terminal.Wsl",
                "startingDirectory": "\\\\wsl$\\Ubuntu\\home\\USER-NAME"
            }
        ]
    }
}
```
