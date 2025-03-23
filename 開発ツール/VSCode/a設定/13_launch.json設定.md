# launch.json 設定

作成日 2025/01/31

公式マニュアル（英語） => [Launch configurations](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)

```json
{
  // IntelliSense を使用して利用可能な属性を学べます。
  // 既存の属性の説明をホバーして表示します。
  // 詳細情報は次を確認してください: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Chrome",
      "request": "launch",
      "type": "chrome",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}"
    },
    {
      "type": "java",
      "name": "DemoApplication",
      "request": "launch",
      "mainClass": "com.example.demo.DemoApplication",
      "projectName": "demo"
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
