# JavaScriptでDOMを操作する

作成日 2021/01/26

## 1. DOM操作のキホン

```javascript
// カレントノードの最後の子として、指定ノードを追加する
parentNode.appendChild(newNode);

// カレントノードの子として、参照先ノードの前に、指定ノードを挿入する
parentNode.insertBefore(newNode, referenceNode);
```

## 2. bodyタグの先端と末尾にそれぞれノードを追加する

```javascript
const h1 = document.createElement('div');
h1.setAttribute('id', 'header');
h1.innerHTML = 'header';

const f1 = document.createElement('div');
f1.setAttribute('id', 'footer');
f1.innerHTML = 'footer';

document.body.insertBefore(h1, document.body.firstChild);
document.body.appendChild(f1);
```
