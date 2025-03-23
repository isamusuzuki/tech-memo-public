# tsconfig.json

作成日 2025/01/06

## moduleResolution

公式サイト（英語） => [TSConfig Reference: Module Resolution](https://www.typescriptlang.org/tsconfig/#moduleResolution)

Specify the module resolution strategy

- `node16` or `nodenext` ... support both `import` and `require`
- `node10` or `node` ... only support CommonJS `require`
- `bundler` ... never requires file extension on relative paths in imports
- `classic` ... should not be used
