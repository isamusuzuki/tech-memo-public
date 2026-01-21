# 開発コンテナからGitプッシュする

作成日 2026/01/21

開発コンテナからGitプッシュを可能にする方法

1. 開発コンテナからGitコマンド＋SSH接続ができるようにパッケージをインストールする
2. ローカル（WindowsまたはMac）で、ssh-agentサービス（デーモンとも言う）を動かし、SSH秘密鍵を渡す

## 1. 開発コンテナに必要なパッケージをインストールする

Dockerfile

```Dockerfile
FROM node:24-trixie-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    openssh-client \
    && rm -rf /var/lib/apt/lists/*
```

## 2. PowerShellでssh-agentを動かすまでの手順

ssh-agentサービスを起動しようと、コマンドを叩くが、失敗する

```bash
ssh-agent
# unable to start ssh-agent service, error :1058
```

それはssh-agentサービスが止まっているから

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

ssh-agentサービスを起動できるように設定を変更する必要がある

管理者モードでPowerShellを動かす

```bash
Get-Service -Name ssh-agent | Set-Service -StartupType Manual


Get-Service ssh-agent | Select StartType
# StartType
# ---------
#   Manual
```

通常のPowerShellに戻る

ssh-agentサービスを起動する

```bash
ssh-agent
# 返事がないのは、正常な証拠

Get-Service ssh-agent
# Status   Name               DisplayName
# ------   ----               -----------
# Running  ssh-agent          OpenSSH Authentication Agent
```

ssh-agentサービスにSSH秘密鍵を渡す

```bash
ssh-add
# Identity added: C:\Users\{User}/.ssh/id_rsa
```

## 3. 注意事項

ssh-agentサービスは、自由度が高い（＝取り扱いに注意が必要な）サービスなので、起動時に自動でスタートすることはできない

VSCodeで開発を行う（開発コンテナを使う）前に、毎回、以下のコマンドを叩く必要がある

```bash
ssh-agent
ssh-add
```
