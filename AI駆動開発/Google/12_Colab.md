# Colab

作成日 2025/04/22

## 1. 有料プランあり

[Colab 有料サービスの料金](https://colab.research.google.com/signup?hl=ja)

- Colab Pro ... 1ヶ月あたり1,179円、100コンピューティングユニット（有効期限90日間）
- Colab Pro+ ... 1ヶ月あたり5,767円、500コンピューティングユニット（有効期限90日間）
- Pay As You Go ... 100コンピューティングユニット 1,179円、500コンピューティングユニット 5,767円（有効期限90日間）

## 2. 選べる GPU

[有料プランの Google Colab の GPU ってどれを使えばいいの？](https://note.com/great_ai_jay/n/n25a888a3e598)

- T4 GPU ... VRAM 16GB, RAM 50.99GB 1時間あたり約1.84クレジット消費
- V100 GPU ... VRAM 16GB, RAM 50.99GB 1時間あたり約1.84クレジット消費
- L4 GPU ... VRAM 24GB, RAM 62.80GB 1時間あたり約4.82クレジット消費
- A100 GPU ... VRAM 40GB, RAM 83.48GB 1時間あたり約11.77クレジット消費

2024年04月19日現在の情報

## 3. Gemma 3 を Colab で試す

[Gemma 3 を Google Colab で試しました](https://note.com/owlet_notes/n/nae84742edee3)

- Gemma 3は、10億、40億、120億、270億（1B、4B、12B、27B）のパラメータを持つ4つのサイズがある
- 各サイズに、事前学習済みモデル（pt：pretrained）と命令チューニング済み（it：instruction-tuned）モデルがある
- 40億、120億、270億パラメータモデルで、画像とテキストの両方を処理できるマルチモーダル機能が追加

| Model          | Colab GPU | Result             |
| -------------- | --------- | ------------------ |
| gemma-3-1b-it  | L4 GPU    | 動作               |
| gemma-3-4b-it  | L4 GPU    | 動作               |
| gemma-3-12b-it | A100 GPU  | 動作               |
| gemma-3-27b-it | A100 GPU  | メモリ不足でエラー |

### 3a.  Python サンプルコード

```bash
!pip install -U accelerate bitsandbytes torch huggingface_hub
!pip uninstall -y transformers
!pip install git+https://github.com/huggingface/transformers.git
```

```python
import os
from huggingface_hub import login

# Hugging Face トークンでログイン
token = os.getenv("HUGGING_FACE_TOKEN")
login(token=token)
```

```python
# 必要なライブラリをインポート
from transformers import pipeline, BitsAndBytesConfig
import torch

# BitsAndBytesを使用してモデルを4bit量子化してロードすることで、GPUのVRAM消費を大幅に削減する
quantization_config = BitsAndBytesConfig(load_in_4bit=True)

# image-text-to-textパイプラインを作成し、Gemmaの最新のマルチモーダルLLMをロード
# torch_dtypeをbfloat16に設定し、計算速度とメモリ効率を改善
pipe = pipeline(
    "image-text-to-text",                  # マルチモーダル（画像＋テキスト）用パイプライン
    model="google/gemma-3-4b-it",        # 使用するモデル名
    torch_dtype=torch.bfloat16,            # メモリ効率向上のために精度を設定
    quantization_config=quantization_config  # 4bit量子化でVRAM使用量を削減
)

# モデルへの入力テキストを設定（質問の例）
# input_text = "How are you? Tell me about yourself."
# 日本語で推論性能をテストするための質問
input_text ="今日の降水確率は50％です。傘を持って行くべでしょうか？ それともレインウェアを持っていくべきでしょうか？もしくは雨が降らないと思っていいでしょうか？日本語で500文字程度で回答してください。"


# パイプラインを用いて入力テキストを処理し、回答を生成
# max_length: 出力される最大トークン数（長さ）
# truncation=True: 入力が長すぎる場合に切り捨てる
response = pipe(input_text, max_new_tokens=512, truncation=True)

# 生成されたテキストを表示
print(response[0]['generated_text'])
```
