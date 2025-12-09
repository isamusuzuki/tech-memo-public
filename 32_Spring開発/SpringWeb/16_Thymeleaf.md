# Thymeleafテンプレートエンジン

作成日 2025/06/18

## 1. 公式チュートリアル（日本語）を読む

[Tutorial: Using Thymeleaf (ja)](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf_ja.html)

- ダイアレクト: テンプレートファイルにデータを埋め込むための文法
- スタンダードダイアレクト: Thymeleafのコアライブラリが提供するダイアレクト
- プロセッサー: ダイアレクトを構成する装置
- 属性プロセッサー: HTMLタグの属性の形をしたプロセッサー

## 2. 属性プロセッサーの例

- `th:text`
- `th:object`
- `th:class`
- `th:each`
- `th:if`
- `th:href` ... linkタグ用
- `th:action` ... formタグ用
- `th:field` ... inputタグ用

## 3. スタンダード式構文

- `${...}` ... 変数式
- `*{...}` ... アスタリスク構文（選択されたオブジェクトに足して式を評価する）
- `@{...}` ... リンクURL式
