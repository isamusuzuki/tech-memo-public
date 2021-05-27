# WSLをセットアップする

作成日 2021/05/15、更新日 2021/05/27

下記の「手動インストールの手順」に従う

[Windows Subsystem for Linux \(WSL\) を Windows 10 にインストールする \| Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10)

- 手順 1 - Linux 用 Windows サブシステムを有効にする
- 手順 2 - WSL 2 の実行に関する要件を確認する
- 手順 3 - 仮想マシンの機能を有効にする
- 手順 4 - Linux カーネル更新プログラム パッケージをダウンロードする
- 手順 5 - WSL 2 を既定のバージョンとして設定する
- 手順 6 - 選択した Linux ディストリビューションをインストールする

## 管理者としてPowerShellを開き、以下を実行する

```bash
# 手順 1
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 手順 3
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# 手順 5
wsl --set-default-version 2
```
