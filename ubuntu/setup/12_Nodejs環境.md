# Ubuntu に Node.js 環境を構築する

作成日 2020/02/26、更新日 2020/10/07

## 01. Node.js 環境を整える

バイナリ配布の公式サイト => [NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions)

```bash
node -v
# => なし

sudo apt install curl
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt install -y nodejs

node -v
# => v12.19.0

npm -v
# => 6.14.8
```
