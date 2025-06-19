# llama.cpp

作成日 2025/04/14、更新日 2025/06/19

## 1. 概要

ローカルでLLMを実行する際に最も人気のあるツール

ソースコード => [ggml-org/llama.cpp: LLM inference in C/C++](https://github.com/ggml-org/llama.cpp)

### 1a. llama.cppが画像の入力に対応した

[ローカルで各種AIモデルを実行できる無料ソフト「llama.cpp」がマルチモーダル入力をサポートし画像の説明などが可能に](https://gigazine.net/news/20250512-llama-cpp--multimodal-vision-support/)

[Trying out llama.cpp’s new vision support](https://simonwillison.net/2025/May/10/llama-cpp-vision/)

## 2. 参照記事を読む

[【llama.cpp】MacのローカルでDeepSeek-R1を試す](https://zenn.dev/michy/articles/2ae54297657f2c)

llama.cppのビルド

```bash
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp

# mac用にコンパイルする
cmake -B build
cmake --build build --config Release
```

huggingFaceからmmngaさんがgguf化してくれたモデルをダウンロードする

[mmnga/DeepSeek-R1-Distill-Qwen-14B-gguf at main](https://huggingface.co/mmnga/DeepSeek-R1-Distill-Qwen-14B-gguf/tree/main/Q4_K_M)

llama.cpp内のmodelsファイル内に移動させる

llama.cppサーバの起動

```bash
./build/bin/llama-server -m models/DeepSeek-R1-Distill-Qwen-14B-Q4_K_M-00001-of-00001.gguf --port 8080
```

ブラウザで`http://127.0.0.1:8080`にアクセスする

設定メニューを開き、temperatureの設定だけ推奨の0.5-0.7のいずれかに設定すれば完了

## 3. llama.cppを開発で利用するには？

### 3a. ContinueというVSCode機能拡張あり

マーケットプレース => [Continue - Codestral, Claude, and more](https://marketplace.visualstudio.com/items?itemName=Continue.continue)

公式ドキュメント（英語） => [Introduction | Continue](https://docs.continue.dev/)

参照サイト => [ローカルLLMをVSCodeでCursorのように使えるcontinue.devの設定方法](https://note.com/shinao39/n/nc8b580b80f15)

> ローカルLLMで使うには当然ながらOpenAI APIと相互性のあるローカルサーバーを立ち上げている必要があります。おすすめはllama-cpp-pythonです。

ソースコード => [abetlen/llama-cpp-python: Python bindings for llama.cpp](https://github.com/abetlen/llama-cpp-python)

### 3b. OpenAI APIと互換性のあるローカルサーバーを立ち上げる

参照サイト => [機密情報も安心？ローカル実行可能なLLMでvscode開発環境を作る](https://qiita.com/kota33/items/63ba76dee2535374af0d)

> llama.cppは、CPUのみでLLMを動かすことができるライブラリです。\
> llama.cppではGGUFというフォーマットのモデルしか利用できません。ダウンロードするモデルはGGUFを選択するか、モデルの変換処理を行ってください。
