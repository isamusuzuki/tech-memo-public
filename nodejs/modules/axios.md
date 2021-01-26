# axios

作成日 2021/01/26

公式サイト => [axios/axios: Promise based HTTP client for the browser and node\.js](https://github.com/axios/axios)

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
