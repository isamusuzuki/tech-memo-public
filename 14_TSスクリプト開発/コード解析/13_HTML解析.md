# HTMLを構造解析する

作成日 2025/07/03、更新日 2025/07/10

## 1. posthtml-parserを試す

[posthtml/posthtml-parser: Parse HTML/XML to PostHTMLTree](https://github.com/posthtml/posthtml-parser)

インストール => `bun add posthtml-parser`

```javascript
import { parser } from 'posthtml-parser';

// HTMLのASTを生成する
const astHtml = parser(code);
```

## 2. HTMLのASTの型定義を調べる

`node_modules/posthtml-parser/dist/index.d.ts`

```javascript
type Content = NodeText | Array<Node | Node[]>;
type NodeText = string | number;
type Node = NodeText | NodeTag;
type NodeTag = {
    tag?: Tag;
    attrs?: Attributes;
    content?: Content;
    location?: SourceLocation;
};
```

- Contentは、NodeTextまたは、NodeまたはNodeのリストの配列である
- NodeTextは、文字列または数字である
- Nodeは、NodeTextまたはNodeTagである
- NodeTagは、Contentを持つ

## 2. HTMLのASTから特定のHTMLタグを抽出する

```javascript
type JsonNode = any;
type ButtonNode = {
    name: string;
    methodName: string;
}

export function findButtonNodes(node: JsonNode, results: ButtonNode[] = []): ButtonNode[] {
    if (Array.isArray(node)) {
        // リストの場合はその要素を探索
        for (const item of node) {
            findButtonNodes(item, results);
        }
    } else if (typeof node === 'object' && node !== null && node.tag && typeof node.tag === 'string') {
        if (node.tag.match(/Button$/)) {
            const button: ButtonNode = {
                name: node.tag || '',
                methodName: node.attrs["@click"] || ''
            };
            results.push(button);
        }
        // 子要素を再帰的に探索
        if (node.content) {
            findButtonNodes(node.content, results);
        }
    }
    return results;
}
```
