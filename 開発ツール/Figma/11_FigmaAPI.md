# Figma API

作成日 20205/05/07

## 1. API、プラグイン、ウィジェット

参照サイト => [現場で活躍する Figma API、プラグイン、ウィジェット](https://zenn.dev/seya/articles/924aadf933034d)

> Figma は UI デザインツールとデファクトスタンダードの地位を確立してきました。\
> そんな Figma にはコードを使って Figma 上の情報を取得してゴニョゴニョできる手段が３つ用意されています。\
> API、プラグイン、ウィジェットの３つです。

## 2. API を実行してみる

> これを実行すると無事コンソールに Figma ファイルのスタイルのデータが表示されていると思います！

```javascript
const baseUrl = 'https://api.figma.com/v1/';
const figmaToken = '先ほど取得した Figma トークン';
const fileId = 'さっき取得したファイルID';

const fetchStylesData = async () => {
  const res = await fetch(`${baseUrl}files/${fileId}/styles`, {
    headers: {
      'X-Figma-Token': figmaToken,
    },
  });
  return await res.json();
};

fetchStylesData().then(console.log);
```

> ただこのデータにはスタイルの ID や type が取得できているだけで具体的なプロパティは取れていません。(font-size とか color とか)\
> なので、次に取得した ID を元にスタイルのノードを取ります。

```javascript
const fetchStyleNodes = async nodeIds => {
  const res = await fetch(`${baseUrl}files/${fileId}/nodes?ids=${nodeIds.join()}`, {
    headers: {
      'X-Figma-Token': figmaToken,
    },
  });
  return await res.json();
};
```

> ここからはもはやあまり Figma API 関係なくただのプログラミングです。

```javascript
// タイポグラフィのスタイルの ID の配列を取得し
const typographyStyleIds = styles.filter(style => style.style_type === 'TEXT').map(style => style.node_id);

// 具体的なデータを取得します
const typographyStyleData = await fetchStyleNodes(typographyStyleIds);

const writeTypographyStyles = async typographyStyleData => {
  const typographyStyles = Object.values(typographyStyleData.nodes).map(node => {
    return {
      // 私が働いているところでは '/' 区切りのネーミングルールなので、これで分割して '-' で繋いでいます
      // この辺りはご自身の Figma のネーミングルールでよしなに調整してください
      styleName: node.document.name
        .split('/')
        .join('-')
        .toLowerCase(),
      ...node.document.style,
    };
  }); // ここは弊社で使っている stitches というライブラリに合わせた書き方になっています。

  // ご自身の CSS の書き方でいい感じに書き換えてください！
  const stitchesString = `
type TypographyNames = ${typographyStyles.map(style => `'${style.styleName}'`).join(' | ')};

export const typographySettings = (value: TypographyNames) => {
      switch (value) {
${typographyStyles
  .map(style => {
    return `        case '${style.styleName}':
          return {
            fontFamily: "${style.fontFamily}",
            fontSize: "${style.fontSize / REM_SIZE}rem",
            letterSpacing: ${style.letterSpacing},
            lineHeight: "${style.lineHeightPx / REM_SIZE}rem",
            fontWeight: "${style.fontWeight}"
          };`;
  })
  .join('\n')}
        default:
          return {
            fontSize: 14,
          };
      }
    }
`;

  // 文字列ができたら最後にファイルに書き込みましょう！
  fs.writeFileSync(`./src/styles/typographySettings.ts`, stitchesString);
};
```
