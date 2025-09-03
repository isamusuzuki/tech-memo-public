# grep コマンド

作成日 2020/02/03

```bash
# grepコマンドの実行結果を引数にして、headコマンドを実行
grep -l bash /etc/* 2> /dev/null | xargs head -3
```
