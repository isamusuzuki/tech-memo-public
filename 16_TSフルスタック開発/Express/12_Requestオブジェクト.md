# Requestオブジェクトを使う

作成日 2021/01/17

[http://expressjs.com/en/api.html#req](http://expressjs.com/en/api.html#req)

Requestオブジェクトを使って情報を収集する

```javascript
app.get('/info', (req, res) => {
  const info = {
    hostname: req.hostname,
    ip: req.ip,
    originalUrl: req.originalUrl,
    params: req.params,
    path: req.path,
    protocol: req.protocol,
    query: req.query,
  };
  res.send(JSON.stringify(info, null, 2));
});
```

`http://localhost:3080/info?foo=bar`をブラウザで開いてみたときの表示

```json
{
  "hostname": "localhost",
  "ip": "::1",
  "originalUrl": "/info?foo=bar",
  "params": {},
  "path": "/info",
  "protocol": "http",
  "query": { "foo": "bar" }
}
```
