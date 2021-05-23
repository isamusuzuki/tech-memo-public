# Create React App

作成日 2021/05/14

## 01. Create React App とは

公式サイト => [Create React App](https://create-react-app.dev/)

公式リポジトリ => [facebook/create\-react\-app: Set up a modern web app by running one command\.](https://github.com/facebook/create-react-app)

## 02. 実際に create-react-app コマンドを使う

```bash
npx create-react-app avocado
cd avocado
npm start
```

事前のインストールは不要。npx コマンドは常に最新のバージョンを取得してきてから、そのコマンドを使う

![React App](images/react-app.png)

### 出来上がったアプリのファイル・フォルダ構造

```text
--avocado/
    |--node_modules/
    |--public/
    |   |--favicon.ico
    |   |--index.html
    |   |--logo192.png
    |   |--logo512.png
    |   |--manifest.json
    |   `--robots.txt
    |--src/
    |   |--App.css
    |   |--App.js
    |   |--App.test.js
    |   |--index.css
    |   |--log.svg
    |   |--reportWebVitals.js
    |   `--setupTests.js
    |--.gitignore
    |--package-lock.json
    |--package.json
    `--README.md
```

### インストールされたパッケージ

package.json の一部

```json
{
  "dependencies": {
    "@testing-library/jest-dom": "^5.12.0",
    "@testing-library/react": "^11.2.7",
    "@testing-library/user-event": "^12.8.3",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "web-vitals": "^1.1.2"
  }
}
```

### 登録済みのスクリプト

package.json の一部

```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
```

react-scripts が create-react-app コマンドの本体であることがわかった

[https://www.npmjs.com/package/react-scripts](https://www.npmjs.com/package/react-scripts)

> This package includes scripts and configuration used by Create React App.

## 03. TypeScript を使うには

[Adding TypeScript \| Create React App](https://create-react-app.dev/docs/adding-typescript/)

```bash
npx create-react-app my-app --template typescript
```
