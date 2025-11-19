# Google ColabのVSCode機能拡張がリリースされた

作成日 2025/11/19

## 1. 解説記事を読む

[VS Code の Google Colab extension の概要｜npaka](https://note.com/npaka/n/n0f2308fe8d57)

数回クリックするだけで使い始めることができます。

(1) 「VSCode」の「Extensions」で「Google Colab」を検索してインストール。

(2) メニュー「ファイル → 新規作成 → ノートブック」でノートブックを作成。

(3) 右上の「カーネルの選択」で「Colab → New Colab Server → GPT → T4 → <エイリアス> → Python3」をクリック。

(4) 動作確認。

```bash
!nvidia-smi
```

nvidia-smiは、NVIDIA製GPUの監視・管理を行うためのコマンドラインツール

## 2. Marketplaceのサイト（英語）を読む

[Colab - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Google.colab)

> Colab is a hosted Jupyter Notebook service that requires no setup to use and provides free access to computing resources, including GPUs and TPUs. Built atop the Jupyter extension, this extension exposes Colab servers directly in VS Code!
