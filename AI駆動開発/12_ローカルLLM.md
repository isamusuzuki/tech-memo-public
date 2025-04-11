# ローカル LLM

作成日 2025/03/06、更新日 2025/04/11

## 1. VSCode の機能拡張に Continue あり

マーケットプレース => [Continue - Codestral, Claude, and more](https://marketplace.visualstudio.com/items?itemName=Continue.continue)

公式ドキュメント（英語） => [Introduction | Continue](https://docs.continue.dev/)

参照サイト => [ローカル LLM を VSCode で Cursor のように使える continue.dev の設定方法](https://note.com/shinao39/n/nc8b580b80f15)

> ローカルLLMで使うには当然ながらOpenAI APIと相互性のあるローカルサーバーを立ち上げている必要があります。おすすめはllama-cpp-pythonです。

### 2. OpenAI API と互換性のあるローカルサーバーを立ち上げる

参照サイト => [機密情報も安心？ローカル実行可能な LLM で vscode 開発環境を作る](https://qiita.com/kota33/items/63ba76dee2535374af0d)

> llama.cppは、CPUのみでLLMを動かすことができるライブラリです。\
> llama.cppではGGUFというフォーマットのモデルしか利用できません。ダウンロードするモデルはGGUFを選択するか、モデルの変換処理を行ってください。

## 3. llama.cpp

ローカルで LLM を実行する際に最も人気のあるツール

ソースコード => [ggml-org/llama.cpp: LLM inference in C/C++](https://github.com/ggml-org/llama.cpp)

## 4. Ollama

ホームページ => [Ollama](https://ollama.com/)

参照サイト => [【Ollama】ローカル環境でLLM推論が可能！特徴や使い方を徹底解説](https://weel.co.jp/media/tech/ollama/)

> 「Ollama」はllama.cppをバックエンドにしたオープンソースソフトウェア（OSS）です。

参照サイト 2 => [Ollama を試すためのModelに関する知識と選び方](https://ishikawa-pro.hatenablog.com/entry/2025/02/12/103919)

> Ollama では GGUF という形式の、 Model を扱うためのファイルを利用します。GGUF はバイナリファイルで、モデルの重みや、さまざまなメタデータや、量子化に関する情報を持っています。基本的には、 Ollama の公式ページに公開されている GGUF 形式の Model を Pull して利用することになります。

パラメーター数

> パラメーター数は多いほど頭がいいです。しかし、パラメーター数が多ければ多いほど、必要な VRAM (GPU のメモリ) も多くなります。
