# TailWind CSSとは

作成日 2026/01/19

## 1. Tailwind CSSを説明する

今流行中のCSSフレームワーク

HTMLのクラス属性に、直接「`p-4`」や「`text-center`」のような小さな機能を持つクラスを書き込んで、Webサイトのデザインを組み立てる

## 2. 従来のCSSとの違い

### 従来のやり方

HTMLタグにクラス名を与える

```html
<button class="button">クリック</button>
```

CSSファイルの中で、クラス名の内容を定義する

```css
button {
    padding: 1rem;
    background-color: blue;
    color: white;
}
```

### Tailwind CSSのやり方

```html
<button class="p-4 bg-blue-500 text-white rounded-lg">クリック</button>
```
