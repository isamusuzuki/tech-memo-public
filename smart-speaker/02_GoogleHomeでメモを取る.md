# Google Home でメモを取る方法

作成日 2020/01/25

## 01. 解説記事を読む

[Google Home に話しかけてメモを取るのが便利すぎる。4 つの方法とその理由 \| BENRI LIFE](https://www.benrilife.com/entry/google-home-memo)

### a. ショッピングリストを利用する

[https://shoppinglist.google.com/](https://shoppinglist.google.com/)

「OK Google！旅行の写真加工を後でやるをショッピングリストに追加して！」

「OK Google！ショッピングリストを開いて」

### b. マイアクティビティ」のログをメモ帳がわりにする

Google Home もしくは Google アシスタントアプリの右上にある自分アイコンから、マイアクティビティページに行ける。このページには検索機能がある

### c. IFTTT でサービスと接続させる

IFTTT Top page > My Applets > New Applet > Click "+this"\

> Find "Google Assistant"\
> Choose "Say a phrase with a text ingredient" trigger\

- What do you want to say? ... メモ \$
- What do you want the Assiatant to say in response? ... メモしました

> Click "+that" > Find "Evernote" > Connect Evernote service\
> Choose "Create a note" action\

- Title ... メモ
- Body ... `{{TextField}}`
- Notebook ... ノートを入れるノートブックの名前を指定する
- Tags ... タグを指定する

「OK Google！メモ。Google Home に話しかけてメモを取るのが便利。」

IFTTT の欠点: ショッピングリストやマイアクティビティの方が、比較的長文メモの認識率が高かった

### d. 「覚えておいて」コマンドを使う

> Google Home(Google Assistant)の機能として、「◯◯ を覚えておいて」と伝えると、「わかりました、次のように覚えておきます、◯◯」と話し、その ◯◯ を質問すればそれを回答してくれるというメモ的な機能があります。

「OK Google！私は古いパソコンをクローゼットの下の段にしまったと覚えておいて！」
「OK Google！私の古いパソコンはどこ？」

「覚えておいて」コマンドの欠点: 覚えてもらった内容を返してもらうための質問を考える手間がかかる

## 02. Any.do アプリに期待する

[The Best To do list App for Google Assistant \| Any\.do](https://www.any.do/to-do-list-app-for-google-assistant/)

[Any\.do \+ Google Assistant \| Any\.do Help Center](https://support.any.do/anydo-google-assistant/)

> Note: The Google Assistant integration is currently available for operating systems running in English only.

Google アシスタントアプリ ＞ 右下のコンパスアイコン ＞ 右上の自分アイコン\
＞ 設定 ＞ サービスタブ\
英語版は、ここに "Notes and Lists"がある。日本語版にはない
