# axios

作成日 2021/01/26

Promise ベースの HTTP クライアント

公式サイト => [axios/axios: Promise based HTTP client for the browser and node\.js](https://github.com/axios/axios)

## 01. axios の基本的な使い方

```javascript
function hello() {
  let url =
    'https://abcdefg.execute-api.ap-northeast-1.amazonaws.com/dev/hello';
  let config = {
    headers: { 'x-api-key': 'YqNxvHubJw8M...' },
  };
  axios
    .get(url, config)
    .then((response) => {
      area1.innerHTML = response.data.message;
    })
    .catch((error) => {
      area1.innerHTML = error;
    });
}
```

## axios でよく使う構文

- `axios.get(url[, config])`
- `axios.post(url[, data[,config]])`
- `axios.put(url[, data[, config]])`
- `axios.delete(url[, config])`

実は、これらは全て `axios(config)` のアリアスである
