# Obfuscatorとは

作成日 2025/10/29

## 1. 公式サイト（英語）を読む

[JavaScript Obfuscator Tool](https://obfuscator.io/)

自分のコードをコピーしにくくして、自分の仕事が他人に盗まれないようにする

```javascript
function hi() {
  console.log("Hello World!");
}
hi();
```

```javascript
function _0x9ce8(){var _0x5b6dc1=['862450OFypwr','5747844YfsdDK','711MIjCKV','27926mqzvTw','Hello\x20World!','4WmeDTi','938HmpDzo','2688ZptfWa','52482ndeZUL','3qMdDaz','108383snIjXf','1630PUqQjZ','8606704MyXlWK'];_0x9ce8=function(){return _0x5b6dc1;};return _0x9ce8();}function _0x3c13(_0x11ea76,_0x534bb3){var _0x9ce81d=_0x9ce8();return _0x3c13=function(_0x3c1316,_0x48215b){_0x3c1316=_0x3c1316-0xab;var _0x2ea9a4=_0x9ce81d[_0x3c1316];return _0x2ea9a4;},_0x3c13(_0x11ea76,_0x534bb3);}(function(_0x13be46,_0x4944ec){var _0x28f770=_0x3c13,_0x2bc7af=_0x13be46();while(!![]){try{var _0x202def=parseInt(_0x28f770(0xad))/0x1*(parseInt(_0x28f770(0xaf))/0x2)+-parseInt(_0x28f770(0xb3))/0x3*(parseInt(_0x28f770(0xab))/0x4)+-parseInt(_0x28f770(0xb7))/0x5+-parseInt(_0x28f770(0xb2))/0x6*(-parseInt(_0x28f770(0xb0))/0x7)+-parseInt(_0x28f770(0xb6))/0x8+-parseInt(_0x28f770(0xac))/0x9*(-parseInt(_0x28f770(0xb5))/0xa)+-parseInt(_0x28f770(0xb4))/0xb*(-parseInt(_0x28f770(0xb1))/0xc);if(_0x202def===_0x4944ec)break;else _0x2bc7af['push'](_0x2bc7af['shift']());}catch(_0x167cf5){_0x2bc7af['push'](_0x2bc7af['shift']());}}}(_0x9ce8,0xba2f2));function hi(){var _0x17ef57=_0x3c13;console['log'](_0x17ef57(0xae));}hi();
```

## 2. Viteプラグイン

[vite-plugin-javascript-obfuscator - npm](https://www.npmjs.com/package/vite-plugin-javascript-obfuscator)

インストール => `npm i -D vite-plugin-javascript-obfuscator`

設定 => vite.config.js

```javascript
import { defineConfig } from 'vite';
import obfuscatorPlugin from "vite-plugin-javascript-obfuscator";

export default defineConfig({
  plugins: [
    obfuscatorPlugin({
      options: {
        debugProtection: true,
      },
    }),
  ],
});
```
