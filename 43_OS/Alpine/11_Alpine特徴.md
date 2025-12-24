# Alpine Linuxの特徴

作成日 2025/11/12

## 1. 解説記事aを読む

[超軽量なAlpine Linuxについて調べた #Docker - Qiita](https://qiita.com/ryuichi1208/items/6020cfabc92bd8153654)

- BusyBoxとmusl（※1）をベースとしたLinuxディストリビューション
- /bin配下はすべて/bin/busyboxへのシンボリックリンクとなっている
- パッケージマネージャーはapkを採用
- シェルはashを採用、bashを使いたい場合は別途インストールが必要
- Dockerのイメージに、何も知らずにAlpineをつかうとハマる

※1 glibc（GNU Cライブラリ）に代わる軽量でセキュアな標準Cライブラリ

## 2. 解説記事bを読む

[Alpineを理解したい](https://zenn.dev/tkomatsu/articles/bf26e569cb70e9c34ad9)

- めちゃくちゃ軽いLinuxディストリビューション
- Alpineにはsudoもない。root以外のユーザーで色々試すには管理者権限が必要
- Alpineではashが標準のシェルになる

## 3. apkコマンド

rootユーザーでないとできないコマンド多数

```bash
# インストール済みパッケージの参照
apk info

# パッケージの更新
apk update

# パッケージ9の検索
apk search {name}

# パッケージのインストール
apk add {name}

# パッケージ削除
apk del {name}
```
