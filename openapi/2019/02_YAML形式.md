# YAML 形式

作成日 2019/07/27

構造化データやオブジェクトを文字列に変換したもの。JSON よりも短く書けて人間にも読みやすい

## 1. シーケンス（リスト、配列）

```yaml
# ブロック形式
- A
- B
- C
---
# インライン形式
[A, B, C]
```

## 2. マッピング（連想配列、辞書）

```yaml
A: aaa
B: bbb
C: ccc
```

## 3. シーケンスもマッピングも階層構造にできる

インデントは空白 2 文字

```yaml
- A
  - A1
  - A2
- B
  - B1
  - B2
---
A:
  A1: aaa
  A2: aaaa
B:
  B1: bbb
  B2: bbbb
```

## 4. シーケンスとマッピングを組み合わせて階層構造をつくる

```yaml
- hosts: web
  sudo: yes
  vars:
    - required: ['docker', 'pyyaml']
  tasks:
    - name: docker imageを作成
　　　 docker_image: name="my/app" state=present
```

## 5. その他

- `#`   ... 行コメント
- `---` ... セパレータ、1 つのファイルの中に複数の YAML ドキュメントを埋め込む
