# CDN 縛りの Vuejs 開発

作成日 2018/11/19、更新日 2018/11/28

## 公式日本語ガイドを読む

[はじめに — Vue\.js](https://jp.vuejs.org/v2/guide/)

```html
<!-- 開発バージョン、便利なコンソールの警告が含まれています -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- 本番バージョン、サイズと速度のために最適化されています -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

例えば`<body id="app">`と設定した場合、開発バージョンでは、
`Do not mount Vue to <html> or <body> - mount to normal elements instead.`といった
エラーが表示されるが、本番バージョンではなんのエラーも出さず、そして動かない

リリースするまでは、開発バージョンを使うこと

## 参考記事

[CDN で始める Vue\.js コンポーネント ① \- Qiita](https://qiita.com/omochironn/items/e2bad98d092935628380)

[CDN で始める Vue\.js コンポーネント ② \- Qiita](https://qiita.com/omochironn/items/c58b4dd730f8fa589327)

[CDN で始める Vue\.js コンポーネント ③ \- Qiita](https://qiita.com/omochironn/items/e9a0b644f79a47a451ec)

## 自分で書いてみたコード

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>郵便番号検索</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.7.2/css/bulma.min.css"
    />
  </head>
  <body>
    <div id="app">
      <section class="section">
        <div class="container">
          <div class="box">
            <h1 class="title">郵便番号検索</h1>
            <div class="field">
              <label class="label">郵便番号（数字7桁）を入力してください</label>
              <div class="control">
                <input
                  class="input"
                  type="text"
                  placeholder="1234567"
                  v-model="zipcode"
                  v-on:keyup.enter="search"
                />
              </div>
            </div>
            <!-- /field -->
            <div class="subtitle">{{ result }}</div>
          </div>
          <!-- /box -->
        </div>
        <!-- /container -->
      </section>
      <!-- /section -->
    </div>
    <!-- /app -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          zipcode: "",
          result: ""
        },
        methods: {
          search: function() {
            if (this.zipcode.match(/^\d{7}$/)) {
              const url = `https://sample.com/api/search?zipcode=${
                this.zipcode
              }`;
              axios
                .get(url)
                .then(response => {
                  if (response.data.results !== null) {
                    const r0 = response.data.results[0];
                    this.result = `${r0.address1}${r0.address2}${r0.address3}`;
                  } else {
                    this.result = `${
                      this.zipcode
                    }は正しい郵便番号ではありません`;
                  }
                })
                .catch(error => {
                  this.result = `エラー発生: ${error}`;
                });
            } else {
              this.result = `${this.zipcode}は数字7桁ではありません`;
            }
          }
        }
      });
    </script>
  </body>
</html>
```
