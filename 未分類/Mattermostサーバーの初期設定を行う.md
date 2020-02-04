---
tags: mattermost
---

# Mattermostサーバーの初期設定を行う

作成日 2019/06/06

トップ画面 => `https://mattermost.eg-wms.com/`\
インストールしたバージョンは `v5.11.0`

## 01. 管理者用アカウントを作成する

- email address => i_suzuki@everglowtrading.com
- username => i_suzuki
- password => 1Passwordに保存した

## 02. チームを作成する

- Team Name => エバーグロー貿易
- Team URL => everglow

ここに転送された => `https://mattermost.eg-wms.com/everglow/channels/town-square`

## 03. 最初のシステム設定

左上のハンバーガーメニュー ＞ メインメニュー ＞ System Console\
＞ チームからシステムコンソールのページに切り替わる

以下はすべて左枠メニューのSETTINGSから選ぶ

### GENERAL > Configuration

項目 | 設定
-----|-----
Site URL | `https://mattermost.eg-wms.com`

### GENERAL > Localization

項目 | 設定
-----|-----
Default Server Language | English -> 日本語
Default Client Language | English -> 日本語

### GENERAL > Users and Teams

項目 | 設定
-----|-----
Enable Team Creation | true -> false
Max Users Per Team | 50 -> 500

### GENERAL > Privacy

項目 | 設定
-----|-----
Show Email Address | true -> false

### AUTHENTICATION > Email

項目 | 設定
-----|-----
Enable sign-in with email | true -> false

### NOTIFICATIONS > Email

項目 | 設定
-----|-----
Enable Preview Mode Banner | true -> false

### FILES > Storage

項目 | 設定
-----|-----
File Storage System | Local File System -> Amazon S3
Amazon S3 Bucket | eg-mattermost-media
Amazon S3 Region | ap-northeast-1
Amazon S3 Access Key ID | AKIAV7Q3HLETZAXMKAND
Amazon S3 Endpoint | s3.amazonaws.com
Amazon S3 Secret Access Key | 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

いったんSaveしてから、Test Connectionを試す\
いったんログアウトしてブラウザを閉じる

## 04. アプリケーションを再起動する

```bash=
sudo systemctl stop mattermost
sudo systemctl start mattermost
```

## 05. 画面の日本語化

左上のハンバーガーメニュー ＞ メインメニューを表示 ＞ Account Settings\
＞ Account Settingsダイアログ登場

左枠 ＞ Display ＞ 右枠 ＞ Language\
＞ ドロップダウンリストから「日本語」を選択 ＞ Saveボタン

## 06. 日本語部分一致検索を有効にする

サーバーにSSH接続する

```bash=
mysql -D mattermost -u mmuser -p
#=> Enter password:

```

以下は、MySQLコンソールでの作業

```sql=
show index from Posts;
-- idx_posts_update_at
-- idx_posts_create_at
-- idx_posts_delete_at
-- idx_posts_channel_id
-- idx_posts_root_id
-- idx_posts_user_id
-- idx_posts_is_pinned
-- idx_posts_channel_id_update_at
-- idx_posts_channel_id_update_at
-- idx_posts_channel_id_delete_at_create_at
-- idx_posts_channel_id_delete_at_create_at
-- idx_posts_channel_id_delete_at_create_at
-- idx_posts_message_txt
-- idx_posts_hashtags_txt

ALTER TABLE Posts DROP INDEX idx_posts_message_txt;

ALTER TABLE Posts ADD FULLTEXT INDEX idx_posts_message_txt (`Message`) WITH PARSER ngram;
```

## 07. カスタム絵文字の登録

システムコンソール ＞ カスタマイズ ＞ 絵文字\
＞ カスタム絵文字を有効にする ＞ 無効 => 有効

エバーグロー貿易に切り替える\
＞ メインメニュー ＞ カスタム絵文字 ＞ カスタム絵文字のページ\
＞ カスタム絵文字を追加

### 【参考】絵文字サイト

[Slackmojis \- The Best Custom Slack Emojis](https://slackmojis.com/)

[絵文字ジェネレーター \- Slack 向け絵文字を無料で簡単生成](https://emoji-gen.ninja/#!/)

## 08. チャンネルを固定にしない

[Configuration Settings — Mattermost 5\.11 documentation](https://docs.mattermost.com/administration/config-settings.html#sidebar-organization-experimental)

サーバーにSSH接続する

```bash=
sudo vi /opt/mattermost/config/config.json
# "ExperimentalChannelOrganization": false
# => "ExperimentalChannelOrganization": true

sudo systemctl restart mattermost
```

アカウントの設定 ＞ サイドバー ＞ チャンネルのグループ化とソート
