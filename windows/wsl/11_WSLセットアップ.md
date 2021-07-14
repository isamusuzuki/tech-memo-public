# WSL をセットアップする

作成日 2021/05/15、更新日 2021/07/14

下記の「手動インストールの手順」に従う

[Windows Subsystem for Linux \(WSL\) を Windows 10 にインストールする \| Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10)

- 手順 1 - Linux 用 Windows サブシステムを有効にする
- 手順 2 - WSL 2 の実行に関する要件を確認する
- 手順 3 - 仮想マシンの機能を有効にする
- 手順 4 - Linux カーネル更新プログラム パッケージをダウンロードする
- 手順 5 - WSL 2 を既定のバージョンとして設定する
- 手順 6 - 選択した Linux ディストリビューションをインストールする

## 管理者として PowerShell を開き、以下を実行する

```bash
# 手順 1
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 手順 3
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

- 手順 3 が終わったら、マシンを再起動する
- 手順 4 で以下のパッケージをダウンロードしてインストールする

[x64 マシン用 WSL2 Linux カーネル更新プログラム パッケージ](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

```bash
# 手順 5
wsl --set-default-version 2
```

- 手順 6 で Windows Store に行き、"ubuntu" を検索して、"ubuntu" をインストールする
- 初回起動時に、Ubuntu上でのユーザー名とパスワードの入力が求められる
- 初回起動時は、時間がかかるので気長に待つ
