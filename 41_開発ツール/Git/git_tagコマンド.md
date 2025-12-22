# git tag コマンド

作成日 2025/01/27

## タグを作成する

```bash
# 先頭のコミットにタグを付ける
git tag {tag-name}

# 先頭のコミットにメッセージ付きのタグを付ける
git tag -a {tag-name} -m "message"
```

## タグの一覧を表示する

```bash
git tag
```

## リモートにアップする

```bash
# 特定のタグをアップする
git push origin {tag-name}

# 全てのタグをアップする
git push origin --tags
```

## タグを削除する

```bash
# ローカルのタグを削除する
git tag -d {tag-name}

# リモートのタグを削除する
git push origin -d {tag-name}
```
