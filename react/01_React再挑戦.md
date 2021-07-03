# React 再挑戦

作成日 2021/04/27

## Vue.js と React について思ったこと

Vue.js は、TypeScript を前提としないで開発された経緯があって、とくに Vuex は、TypeScriptでどう設定したら使えるようになるのかまったく理解できなかった。こちらの理解度の低さよりも、無理やり辻褄を合わせようとしているがための難解さを感じた

もし React が、TypeScript を前提に開発されているなら、そちらのほうが理解しやすいのではないかと思った

## 比較記事を読む

[完全に独断と偏見だけど React vs Vue してみた \- Qiita](https://qiita.com/102Design/items/ae018dc80a4d879d92a8)

|                     | React | Vue |
| ------------------- | :---: | :-: |
| 手軽さ              |   △   |  ◎  |
| TypeScript との相性 |   ◎   |  △  |
| GraphQL との相性    |  〇   |  △  |

> こと手軽さにおいては Vue の方が圧倒的と言っていいです。
>
> ただ、ロジック部分はReactの方が優秀。Hooksにより再利用性・拡張性の高いロジックをComponentで使用可能かつ、StateをComponentに閉じ込めることができます。
>
> TypeScriptとの相性・親和性においてはReactの方が優っていると判断しました。

## 再び問う、React とは

公式サイト => [https://ja.reactjs.org/](https://ja.reactjs.org/)

> ユーザインターフェイス構築のための JavaScript ライブラリ

## React の勉強を開始する

公式チュートリアル => [チュートリアル：React の導入 – React](https://ja.reactjs.org/tutorial/tutorial.html)

解説記事 => [TypeScriptでReactに入門するチュートリアル \- Qiita](https://qiita.com/yonetty/items/012be4c5c6258a609e35)
