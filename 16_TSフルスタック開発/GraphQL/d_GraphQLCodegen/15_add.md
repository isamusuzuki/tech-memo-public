# addプラグイン

作成日 2025/10/09

## 1. 公式サイト（英語）を読む

[Plugins > other > add](https://the-guild.dev/graphql/codegen/plugins/other/add)

addプラグインは、出力ファイルにカスタム文字列を追加する

```javascript
import type { CodegenConfig } from '@graphql-codegen/cli'

const config: CodegenConfig = {
  // ...
  generates: {
    'path/to/file.ts': {
      plugins: [
        {
          add: {
            content: '/* eslint-disable */'
          }
        },
        'typescript'
      ]
    }
  }
}
export default config
```
