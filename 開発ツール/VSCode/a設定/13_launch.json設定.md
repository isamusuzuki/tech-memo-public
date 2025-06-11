# launch.json 設定

作成日 2025/01/31、更新日 2025/06/11

## 1. [Launch configurations](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)

launch.jsonの例

```json
{
  // IntelliSense を使用して利用可能な属性を学べます。
  // 既存の属性の説明をホバーして表示します。
  // 詳細情報は次を確認してください: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "java",
      "name": "DemoApplication",
      "request": "launch",
      "mainClass": "com.example.demo.DemoApplication",
      "projectName": "demo"
    },
    {
      "name": "Launch Chrome",
      "request": "launch",
      "type": "chrome",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}"
    }
  ],
  "compounds": [
    {
      "name": "Debug Spring",
      "configurations": ["DemoApplication", "Launch Chrome"],
      "stopAll": true
    }
  ]
}
```

この例で"Debug Spring"を実行すると、サーバーが立ち上がる前に、ブラウザが起動してしまう。これを解消したい

## 2. [Automatically open a URI when debugging a server program](https://code.visualstudio.com/docs/debugtest/debugging-configuration#_automatically-open-a-uri-when-debugging-a-server-program)

Webプログラムを開発していると、デバッグ中のサーバーのコードを叩くために、ブラウザで特定のURLを開く必要がある。VSCodeには、このタスクを自動化する`serverReadyAction`という備え付けの機能がある。

- `pattern` ... プログラムの出力の中で、このパターンが登場したら発射される
- `action` ... `startDebugging`をセットすると、別の設定を起動できる
- `name` ... ここで別の設定を指定する
- `killOnServerStop` ... サーバーのデバッグを停止したら、ブラウザも止めるかどうか

```json
{
  {
  // IntelliSense を使用して利用可能な属性を学べます。
  // 既存の属性の説明をホバーして表示します。
  // 詳細情報は次を確認してください: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "java",
      "name": "DemoApplication",
      "request": "launch",
      "mainClass": "com.example.demo.DemoApplication",
      "projectName": "demo",
      "serverReadyAction": {
        "pattern": "Started DemoApplication in [0-9.]+ seconds",
        "action": "startDebugging",
        "name": "Launch Chrome",
        "killOnServerStop": true
      }
    },
    {
      "name": "Launch Chrome",
      "request": "launch",
      "type": "chrome",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```
