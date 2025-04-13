# Continue

作成日 2025/04/14

## 1. Continueとは

マーケットプレース => [Continue - Codestral, Claude, and more](https://marketplace.visualstudio.com/items?itemName=Continue.continue)

公式ドキュメント（英語） => [Introduction | Continue](https://docs.continue.dev/)

参照サイト => [ローカル LLM を VSCode で Cursor のように使える continue.dev の設定方法](https://note.com/shinao39/n/nc8b580b80f15)

> ローカルLLMで使うには当然ながらOpenAI APIと相互性のあるローカルサーバーを立ち上げている必要があります。おすすめはllama-cpp-pythonです。

### 2. OpenAI API と互換性のあるローカルサーバーを立ち上げる

参照サイト => [機密情報も安心？ローカル実行可能な LLM で vscode 開発環境を作る](https://qiita.com/kota33/items/63ba76dee2535374af0d)

> llama.cppは、CPUのみでLLMを動かすことができるライブラリです。\
> llama.cppではGGUFというフォーマットのモデルしか利用できません。ダウンロードするモデルはGGUFを選択するか、モデルの変換処理を行ってください。
