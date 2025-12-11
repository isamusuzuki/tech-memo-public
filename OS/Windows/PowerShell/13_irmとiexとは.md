# irmとiexとは

作成日 2025/12/11

## Bunのインストールスクリプトを読み解く

```bash
powershell -c "irm bun.sh/install.ps1 | iex"
```

- irm (Invoke-RestMethod) ... ウェブサイトからスクリプトをダウンロードする
- iex (Invoke-Expression) ... スクリプトを実行する

[Invoke-RestMethod (Microsoft.PowerShell.Utility) - PowerShell | Microsoft Learn](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-7.5)

> RESTful Web サービスに HTTP または HTTPS 要求を送信します。

[Invoke-Expression (Microsoft.PowerShell.Utility) - PowerShell | Microsoft Learn](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.5)

> ローカル コンピューターでコマンドまたは式を実行します。
