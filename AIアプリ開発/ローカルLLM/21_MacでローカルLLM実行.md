# MacでローカルLLMを実行する

作成日 2025/05/12

## 1. 参照サイトを読む

[M3mac環境】llama.cppを使ったGGUF変換からOllamaで実行までの手順](https://zenn.dev/k_zumi_dev/articles/1c752e21c6406f)

MacでOllamaを使ってローカルLLMを動作させる

OllamaでLLMを動かすには3種類のパターンがあります。上から順に試してみましょう。

1. ollamaのModelsに公開されているモデルをダウンロードして使う
2. huggingfaceに公開されているGGUFファイルを使う
3. huggingfaceに公開されているSafetensorsファイルをGGUFファイルに変換して使う

### 1a. Ollamaのインストール

まずは以下のページからインストーラをダウンロードして実行します。

[Download Ollama](https://ollama.com/download)

実行できたらmacの上部のバーにラマのアイコンが表示されます。特にウィンドウは立ち上がりません。

ターミナルを開き、以下のコマンドを打ってみましょう。

```bash
ollama --version

# OllamaのHPのモデルの一覧から好きなモデルを探す
# モデルのダウンロードが終わるまで待つと、メッセージ入力欄が現れる
ollama run gemma:2b

# 入力待機状態を終了する
/bye

# インストール済みのモデル一覧を表示する
ollama list

# モデルを削除する
ollama rm gemma:2b
```

### 1b. huggingfaceに公開されているGGUFファイルを使う

Ollamaからhuggingface上のファイルを簡単にダウンロードできるようになっているため、操作自体は簡単です。

実行時のコマンドのモデル名のところに、モデルの公開ページからURLをコピーして「https://」をはずしてペーストするだけです。

```bash
ollama run huggingface.co/elyza/Llama-3-ELYZA-JP-8B-GGUF
```
