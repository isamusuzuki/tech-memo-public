# localStorage を使う

作成日 2019/02/07

- 親コンポーネントは、ほぼストアと子コンポーネントを組み込むだけ
- 親コンポーネントの唯一の仕事は、読み込みのときにストアの load 関数をコミットすること
- localStorage を操作するのはストアだけの仕事
- ストアのミューテーションに localStorage の getItem, setItem, removeItem が並ぶ
- 子コンポーネントは、ストアとコミュニケーションするのみ

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>localstorageデモ</title>
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
          <h1 class="title">localStorageデモ</h1>
          <hello />
        </div>
        <!-- /container -->
      </section>
      <!-- /section -->
    </div>
    <!-- /app -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex/dist/vuex.js"></script>
    <script>
      const hello = {
        data: function() {
          return {
            namae: "",
            message: ""
          };
        },
        methods: {
          koushin: function() {
            this.message = `こんにちは、${this.namae}さん`;
            this.$store.commit("update", this.namae);
          },
          souji: function() {
            this.$store.commit("clear");
            alert("ローカルストレージを掃除しました");
          }
        },
        mounted: function() {
          if (this.$store.state.name) {
            this.namae = this.$store.state.name;
            this.message = `こんにちは、${this.$store.state.name}さん`;
          }
        },
        template: `
                <div class="box">
                    <label class="label">お名前を教えてくださいvuex</label>
                    <div class="control">
                    <div class="columns">
                    <div class="column">
                    <input class="input" type="text" placeholder="お名前" v-model="namae">
                    </div>
                    <div class="column">
                    <a class="button is-primary" v-on:click="koushin">更新</a>
                    <a class="button is-danger" v-on:click="souji">掃除</a>
                    </div>
                    </div>
                    </div>
                    <div class="subtitle">&nbsp;{{ message }}</div>
                </div>
            `
      };

      const store = new Vuex.Store({
        state: {
          name: ""
        },
        mutations: {
          update: function(state, name) {
            state.name = name;
            localStorage.setItem("name123", name);
          },
          load: function(state) {
            if (localStorage.getItem("name123")) {
              state.name = localStorage.getItem("name123");
            }
          },
          clear: function(state) {
            state.name = "";
            localStorage.removeItem("name123");
          }
        }
      });

      const app = new Vue({
        el: "#app",
        store,
        components: {
          hello: hello
        },
        created: function() {
          store.commit("load");
        }
      });
    </script>
  </body>
</html>
```
