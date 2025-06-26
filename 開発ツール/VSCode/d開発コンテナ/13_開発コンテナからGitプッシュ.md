# 開発コンテナからGitプッシュする

作成日 2025/03/25

## 1. 参考記事を読む

参照サイト（日本語） => [devcontainer内でgitを使いたい](https://qiita.com/pappy/items/16dba1469a0d97ce0be6)

> 要は、ローカルでssh-agentが動いていたらdevcontainerでもsshキーが使えますよ、ということらしい。\
> ssh-agentとはsshプロトコルを使用するための認証情報（SSHキーのパスフレーズなど）を管理するデーモンです。

参照サイト（英語） => [Starting ssh-agent in Windows PowerShell](https://peateasea.de/starting-ssh-agent-in-windows-powershell/)

## 2. PowerShellでssh-agentを動かすまでの手順

コマンドを叩くと失敗する

```bash
ssh-agent
# unable to start ssh-agent service, error :1058
```

それはサービスが止まっているから

```bash
Get-Service ssh-agent

# Status   Name               DisplayName
# ------   ----               -----------
# Stopped  ssh-agent          OpenSSH Authentication Agent


Get-Service ssh-agent | Select StartType

# StartType
# ---------
# Disabled
```

サービスを動かすために、管理者モードでPowerShellを動かす

```bash
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Get-Service ssh-agent | Select StartType

# StartType
# ---------
#   Manual

Start-Service ssh-agent
Get-Service ssh-agent

# Status   Name               DisplayName
# ------   ----               -----------
# Running  ssh-agent          OpenSSH Authentication Agent
```

通常モードのPowerShellに戻ってSSH鍵をssh-agentに渡す

```bash
ssh-add
# Identity added: C:\Users\{User}/.ssh/id_rsa
```

これで開発コンテナでもGitプッシュができるようになった
