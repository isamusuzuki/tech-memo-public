# llama.cpp

作成日 2025/04/14

## 1. llama.cppとは

ローカルでLLMを実行する際に最も人気のあるツール

ソースコード => [ggml-org/llama.cpp: LLM inference in C/C++](https://github.com/ggml-org/llama.cpp)

## 2. ローカルLLMを開発に利用する

## 2a. ContinueというVSCode機能拡張あり

マーケットプレース => [Continue - Codestral, Claude, and more](https://marketplace.visualstudio.com/items?itemName=Continue.continue)

公式ドキュメント（英語） => [Introduction | Continue](https://docs.continue.dev/)

参照サイト => [ローカルLLMをVSCodeでCursorのように使えるcontinue.devの設定方法](https://note.com/shinao39/n/nc8b580b80f15)

> ローカルLLMで使うには当然ながらOpenAI APIと相互性のあるローカルサーバーを立ち上げている必要があります。おすすめはllama-cpp-pythonです。

ソースコード => [abetlen/llama-cpp-python: Python bindings for llama.cpp](https://github.com/abetlen/llama-cpp-python)

### 2b. OpenAI APIと互換性のあるローカルサーバーを立ち上げる

参照サイト => [機密情報も安心？ローカル実行可能なLLMでvscode開発環境を作る](https://qiita.com/kota33/items/63ba76dee2535374af0d)

> llama.cppは、CPUのみでLLMを動かすことができるライブラリです。\
> llama.cppではGGUFというフォーマットのモデルしか利用できません。ダウンロードするモデルはGGUFを選択するか、モデルの変換処理を行ってください。
