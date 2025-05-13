# llama.cppを使う

作成日 2025/05/13

## 1. 参照サイトを読む

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
