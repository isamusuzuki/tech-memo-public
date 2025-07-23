# Promise

作成日 2019/04/15

## 1. Promiseとは

JavaScriptは「ノンブロッキングI/O」であり、関数を呼んだ時にそのレスポンスを待たずに、次の行へと進んでいく。axiosによる外部サーバーへのアクセスなど、処理にものすごく時間がかかる関数の場合は、結果が得られたときに実行するコールバック関数を、引数として与えることで対処する。成功したときのコールバック関数と、失敗したときのコールバック関数を、なるたけ自然に書けるようにしたのがPromiseである

## 2. Promiseを設定する側（時間がかかる関数）

新規のPromiseインスタンスを返すように書き、そのインスタンスの引数として関数を書き、その関数の引数として resolveとrejectという関数を与え、関数の本体で、本当にやりたいことを書き、正常終了したときはresolve関数を実行し、異常終了したときはreject関数を実行する

```javascript
// authモジュールのgetTokenアクション関数
getToken (context, payload) {
    return new Promise((resolve, reject) => {
        axios.post(`${context.rootState.settings.serverUrl}/kaigi/auth`,
            {login_name: payload.account, login_password: payload.password}
        ).then(response => {
            // 成功
            const newAuth = {name: payload.account, token: response.data.token}
            context.commit('update', newAuth)
            localStorage.setItem('kaigi-auth', JSON.stringify(newAuth))
            resolve()
        }).catch(error => {
            // 失敗
            reject(error.response)
        });
    });
}
```

## 3. Promiseを使う側

`.then()`でチェーンできる。その中身は関数。その引数は、resolve関数に与えられた引数。この関数は、getTokenアクション関数が正常終了したときに実行される

`.catch()`でチェーンできる。その中身も関数。その引数は、reject関数に与えられた引数。この関数は、getTokenアクション関数が異常終了したときに実行される

```javascript
// loginページのlogin()メソッド
this.$store
  .dispatch('auth/getToken', {
    account: this.loginName,
    password: this.password,
  })
  .then(() => {
    this.$store.dispatch('loader/off');
    this.$router.push({ path: '/home' });
  })
  .catch((error) => {
    this.$store.dispatch('message/error', { messages: ['ログイン失敗'] });
    this.$store.dispatch('loader/off');
  });
```
