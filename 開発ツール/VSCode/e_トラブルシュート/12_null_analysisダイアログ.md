# null analysis ダイアログ

作成日 2025/02/18

## たびたび起こる現象

初めて Java プロジェクトを開くと、以下のダイアログが登場する

```text
Null annotation types have been detected in the project. Do
you wish to enable null analysis for this project?

ソース: Language Support for Java(TM) by Red Hat

[Enable] [Disable]
```

Enable ボタンをクリックすると、`.vscode/settings.json`に設定が追加される

```json
{
  "java.compile.nullAnalysis.mode": "automatic"
}
```

## annotation-based null analysis とは

【自分の理解】`@NonNull`, `@Nullable`アノテーションが使われている時に、エディター（＋機能拡張）がそれに準じた反応をすること
