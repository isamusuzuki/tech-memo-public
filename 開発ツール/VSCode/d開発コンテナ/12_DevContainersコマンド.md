# Dev Containers コマンド

作成日 2025/02/11

コマンドパレットで使えるコマンド集

## 1. `Dev Containers: Get Started with Dev Containers`

- 公式サイト（英語）の開発コンテナ説明ページが開く
- [Developing inside a Container](https://code.visualstudio.com/docs/devcontainers/containers)

## 2. `Dev Containers: Try a Dev Container Sample...`

- 言語ごとに用意されているトライアウトのコンテナが立ち上がる
- コンテナに接続しているので、コードのデバッグやテストが可能
- ローカルは全く汚されない

## 3. `Dev Containers: Add Dev Cotainer Configuration Files...`

質問に答えていくことでコンテナ構成ファイルを作成できる

### 質問＆返答の例

- コンテナ構成ファイルをどこに作成しますか？ ＞ ワークスペースに構成ファイルを追加する
- コンテナ構成テンプレートを選択する ＞ Java ＞ 21-bookworm
- Install Maven, a management tool for Java
- インストールする追加機能 ＞ なし
- オプションのファイル／ディレクトリ ＞ なし

生成された`.devcontainer/devcontainer.json` の内容

```json
{
  "name": "Java",
  "image": "mcr.microsoft.com/devcontainers/java:1-21-bookworm",
  "features": {
    "ghcr.io/devcontainers/features/java:1": {
      "version": "none",
      "installMaven": "true",
      "installGradle": "false"
    }
  }
}
```
