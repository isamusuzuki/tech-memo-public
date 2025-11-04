# Codex

作成日 2025/05/21

## 1. Codexとは

公式サイト => [Codex](https://chatgpt.com/codex)

> A cloud-based software engineering agent that answers codebase questions, executes code, and drafts pull requests.
>
> Available to users on the ChatGPT Pro Plan and ChatGPT Team Plan

参照記事 => [OpenAI Codex のクイックスタート](https://note.com/npaka/n/n955421dfe2d2)

> 「Codex」のセットアップ手順は、次のとおりです。
> (1) ChatGPT左上の「Codex」をクリック。
> (2) 「開始」をクリック。
> (3) 「GitHubを接続する」をクリック。
> (4) コード作成・編集するリポジトリを選択し、「環境を作成する」をクリック。
> (5) 学習を許可するかどうかを選択して、「続ける」をクリック。
>
> 「Codex」の実行手順は、次のとおりです。
> (1) 実行するタスクをチェックして、「Start tasks」をクリック。
> (2) Codexのメイン画面の表示。
> (3) タスクの実行結果の確認。
> (4) コード編集を指示。
> (5) 「プッシュ通知 → PRを作成する」をクリック。

## 2. Codex CLIとの違い

- Codex ... クラウドにあるChatGPTに対して、自分のGitHubリポジトリを公開して、作業をさせる
- Codex CLI ... ローカルにある自分のPCのターミナルに常駐させて、作業をさせる

## 3. 使っているモデルが違うらしい

参照記事 => [【ついにきたか】OpenAI Codex - OpenAIが生み出した天才コーディングエージェント](https://zenn.dev/holy_fox/articles/39a03defbced35)

> Codexの頭脳となっているのは 「codex-1」 と呼ばれる最新モデルで、OpenAIの高度な大規模言語モデル「o3」系列をソフトウェア開発向けに最適化したものです。
> このモデルは実際のコーディングタスクを用いた強化学習（実環境で試行錯誤させてフィードバックする学習法）によって訓練されており、
> 人間の書くコードスタイルやプルリクエストの好みに近い出力を生成します。
> また指示に忠実に従い、テストが全て通るまでコードを繰り返し実行・修正するといった挙動も身につけています。
>
> リポジトリ内に「AGENTS.md」というファイルを用意し、プロジェクト特有の情報を記載するとCodexの精度向上が期待できます。このファイルには
> プロジェクトのディレクトリ構成やビルド方法、テストの実行コマンド、コーディング規約などをまとめておき、エージェントへのガイドラインとして機能させます。
> AGENTS.mdはREADMEに似た存在ですが、Codexに対する「作業の手引き」と位置付けるとよいでしょう。
>
> 「codex-mini-latest」はCodexの小型版モデルで、主にCodex CLIやAPI経由で提供されます。OpenAIの「o4-mini」というモデルをベースに設計されており、
> 対話型のコード質問応答や素早いコード編集提案など、低レイテンシでの開発支援に適したチューニングがされています。codex-1と比べるとモデルサイズが小さい分
> 応答が高速で、ちょっとしたコード疑問の解消やインクリメンタルな編集には最適です。
