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

### 1c. huggingfaceに公開されているSafetensorsファイルをGGUFファイルに変換して使う

Ollamaで利用できるようにするには、(1) GGUF形式のファイル、(2) Ollama Modelfile、の2つが必要です。

llama.cppと言うLLMのライブラリをセットアップします。

```bash
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build --config Release
```

Safetensorsファイル（transformerモデル）のダウンロード

```bash
# GitLFSをインストールしていない場合は、事前にインストールする
brew install git-lfs

# 以下のコマンドでファイルをダウンロードする。
# 時間がかかる上にインジケーターが表示されないので心配になるが、
# ダウンロードが完了するまで待つ。
cd ../
mkdir models
cd models
git lfs install
git clone https://huggingface.co/elyza/Llama-3-ELYZA-JP-8B
```

GGUFモデルに変換

```bash
cd ../
python3 -m venv venv
source venv/bin/activate
python -m pip install -r ./llama.cpp/requirements.txt 

python ./llama.cpp/convert_hf_to_gguf.py ./models/Llama-3-ELYZA-JP-8B/ --outtype f16 --outfile ./models/Llama-3-ELYZA-JP-8B-f16.gguf

# Ollama Modelfileを作成する
cd models
nano Modelfile_Llama-3-ELYZA-JP-8B-f16.txt
```

```bash
FROM ./Llama-3-ELYZA-JP-8B-f16.gguf
TEMPLATE """{{ if .System }}<|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|>{{ end }}{{ if .Prompt }}<|start_header_id|>user<|end_header_id|>

{{ .Prompt }}<|eot_id|>{{ end }}<|start_header_id|>assistant<|end_header_id|>

{{ .Response }}<|eot_id|>"""
PARAMETER stop "<|start_header_id|>"
PARAMETER stop "<|end_header_id|>"
PARAMETER stop "<|eot_id|>"
PARAMETER stop "<|reserved_special_token"
```

Ollamaのモデルを作成

```bash
ollama create Llama-3-ELYZA-JP-8B-f16.gguf -f Modelfile_Llama-3-ELYZA-JP-8B-f16.txt

# 新モデルを確認
ollama list
# NAME
# Llama-3-ELYZA-JP-8B-f16.gguf:latest

# 実行
ollama run Llama-3-ELYZA-JP-8B-f16.gguf:latest
```
