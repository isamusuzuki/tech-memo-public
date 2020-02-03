# kintone をカスタマイズする

作成日 2019/09/18

## 01. 使用中のアプリにどんなカスタマイズがされているか調べる

kintone アプリ ＞ 右上の歯車アイコン ＞ 設定タブ\
＞ 中段の「JavaScript/CSS でカスタマイズ」 ＞ ここにいろいろな js ファイルが登録されている

たくさん js ファイルが登録されていたが、どうも日付関連の共通ファイルのようだ。自分で開発したようなファイルは 1 つしかなかった

```javascript
(function() {
    'use strict';
    kintone.event.on(
        ['app.record.edit.submit', 'app.record.index.edit.submit'],
        function(e) {
            var record = e.record;
            // データ完了日に値が入っていて、
            // 出品完了日に値が入っていなかったら、
            // 出品完了日にデータ完了日を入れる
        }
    );
})();
```

## 02. チュートリアルを読む

[kintone カスタマイズ チュートリアルの進め方 – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/360024370392)

左枠に「はじめよう JavaScript」コーナーがある

### はじめよう JavaScript

第 12 回からちゃんと読む

[はじめよう JavaScript 第 12 回 kintone JavaScript カスタマイズで kintone のデータを見てみる – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/360000903686)

```javascript
(function() {
    'use strict';
    kintone.events.on('app.record.detail.show', function(e) {
        var record = e.record;
        console.log('------------------------');
        console.log('会社名:', record.会社名.value);
        console.log('部署名:', record.部署名.value);
        console.log('担当者名:', record.担当者名.value);
    });
})();
```

-   `kintone.events.on()`は、指定されたイベント時に動作する関数を登録する
-   `app.record.detailshow`は、「詳細画面が表示されたとき」を表す
-   `var record = e.record;`で、レコードデータを取得する
-   変数`e`にどんなデータが含まれているかは、次のページに記載がある

[レコード表示イベント – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/201941974)

レコード詳細画面が表示された時のイベント

| name     | type   | comment              |
| -------- | ------ | -------------------- |
| appId    | int    | アプリ ID            |
| record   | Object | レコードオブジェクト |
| recordId | int    | レコード ID          |

## 03. API ドキュメントを読む

[kintone JavaScript API 一覧 – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/360000361686)

### イベント

| 関数                             | タイミング                  |
| -------------------------------- | --------------------------- |
| app.record.create.submit.success | レコード追加画面 保存成功後 |
| app.record.edit.submit.success   | レコード編集画面 保存成功後 |

### API 実行

| 関数          | 説明            |
| ------------- | --------------- |
| kintone.api   | API 実行        |
| kintone.proxy | 外部 API の実行 |

## 04. Webhook とは

kintone アプリの設定画面には、「JavaScript/CSS でカスタマイズ」とは別に「Webhook」という項目がある\
Webhook とは、レコードの追加や更新などの操作が行われたときに、外部サービスに通知を送信する機能

### ヘルプを読む

[Webhook を設定する \- kintone ヘルプ](https://jp.cybozu.help/k/ja/user/app_settings/set_webhook/webhook.html)

> kintone で Webhook を設定すると、次の操作が行われたときに、そのことを外部の Web サービスに通知できます。
>
> -   レコードの追加
> -   レコードの編集
> -   レコードの削除
> -   コメントの書き込み
> -   レコードのステータスの変更
>
> 注意\
> 次の方法でレコードを操作した場合は、Webhook の通知は送信されません。
>
> -   Excel/CSV ファイルを読み込んでレコードを操作する
> -   複数のレコードを一括削除する
> -   複数のレコードを一括操作する REST API を使用してレコードを操作する

大事なポイント => CSV インポートでは Webhook の通知が送信されない
