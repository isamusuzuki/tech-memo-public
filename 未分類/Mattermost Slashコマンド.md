---
tags: mattermost
---

# Mattermost Slashコマンド

## ドキュメントを読む

Adminドキュメント => [Slash Commands](https://docs.mattermost.com/developer/slash-commands.html)

開発者ドキュメント => [Slash Commands](https://developers.mattermost.com/integrate/slash-commands/)

### 統合機能で「Slashコマンド」を追加したときの入力項目

- タイトル
- 説明
- コマンドトリガーワード
- リクエストURL
- 返信ユーザー名
- 応答アイコン
- 自動補完

### Slashコマンドが送信するデータの例

```=
POST / HTTP/1.1
Host: 127.0.0.1:8080
User-Agent: mattermost-5.9.0
Content-Length: 548
Accept: application/json
Authorization: Token nezum4kpu3faiec7r7c5zt6tfy
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip

channel_id=i3bb9xfyqt8rtbyshmyhgsj16c&
channel_name=town-square&
command=%2Fexample&
response_url=http%3A%2F%2F10.0.0.5%3A8065%2Fhooks%2Fcommands%2Fzozc1xwxybdedeyz8djwjpngny&
team_domain=rrrr&
team_id=tsb8crrn5tgqtedpkt81b4tcya&
text=&
token=nezum4kpu3faiec7r7c5zt6tfy&
trigger_id=NG1kM3lyN2NqYmQxcGNyc2s0Nmo5em0xb2M6azF4NGFxZGp5MzgxM2M4NG03NzFlb2M5eG86MTU1MTIwODE5NTQyNzpNRVVDSUhSdWFrdmVGZ0RhTTd6UERoMWVEVndZK2NGbXlSYUxWQ054SVRLZGdxTWZBaUVBeGQvOU95NTFOeWxiTWVsRE1ZK0d4S2FzL2Z1TUU2Y0J1bW5JbFBCOXVEVT0%3D&
user_id=k1x4aqdjy3813c84m771eoc9xo&
user_name=tester
```








