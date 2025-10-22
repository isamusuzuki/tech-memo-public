# SemVer表記

作成日 2025/10/22

## 1. 公式ガイド（英語）を読む

[About semantic versioning | npm Docs](https://docs.npmjs.com/about-semantic-versioning)

- `~1.0.4` ... Patch Releaseを受け付ける
- `^1.0.4` ... Minor Releaseを受け付ける
- `*` ... Major Releaseを受け付ける

## 2. SemVerカリキュレーターを使ってみる

[npm semantic version calculator](https://semver.npmjs.com/)

Package nameが`lodash`のまま、Version Rangeをいろいろ書き換えてみる

- `*` ... 0.1.0から4.17.21まで110個のバージョンが登場
- `^4.17.2` ... 4.17.2から4.17.21まで17個のバージョンが登場
- `^4.16.0` ... 4.16.0から4.17.21まで26個のバージョンが登場
- `^3.0.0` ... 3.0.0から3.10.1まで17個のバージョンが登場
- `~4.17.2` ... 4.17.2から4.17.21まで17個のバージョンが登場
- `~4.16.0` ... 4.16.0から4.16.6まで7個のバージョンが登場
- `~3.0.0` ... 3.0.0から3.0.1まで2個のバージョンが登場
