# Responseオブジェクトを使う

作成日 2021/01/17

[http://expressjs.com/en/api.html#res](http://expressjs.com/en/api.html#res)

Reponseオブジェクトを使って返事をする

```javascript
res.send('Hello, World!')
res.status(404).send('404 Not Found')

res.sendFile(path.join(__dirname, 'templates/index.html'))

res.setHeader('Content-Type', 'text/plain; charset=utf-8')
res.write('受信したデータは次の通り\n')
res.write(data)
res.end('以上')

res.json({ user: 'tobi' })
res.status(500).json({ error: 'message' })

res.redirect(301, 'http://example.com')
```
