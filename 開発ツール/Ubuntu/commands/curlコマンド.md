# curl コマンド

作成日 2020/02/03、更新日 2020/04/28

## よく使う curl コマンドのオプション

```bash
# HTTPヘッダを出力に含める
curl -i http://www.example.com/

# POSTリクエストとしてフォームを送信する
curl -d '{"login_id":"i_suzuki"}' http://www.example.com/

# HTTPヘッダを追加する
curl -H 'Authorization: Bearer 58coh66' http://www.example.com/
```

## たまに使う　curl コマンドのオプション

```bash
# リダイレクトに追随する
curl -L http://www.example.com/

# ヘッダーを取得する
curl -I http://www.example.com/

# 詳細なログを出力する
curl -v http://www.example.com/
```
