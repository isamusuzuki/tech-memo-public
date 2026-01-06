# Cloudflareを使って無料サーバーを立てる

作成日 2026/01/06

## 1. Cloudflareのホームページでの作業

- [Cloudflare](https://www.cloudflare.com/ja-jp/)に行き、無料でアカウントを作成する
- ”Workers & Pages”で、サブドメインを決める
- "Account ID"をメモする
- アカウント管理 ＞ "アカウントAPIトークン"で、APIトークンを作成する

## 2. GitHubのホームページでの作業

- 新しいリポジトリを作成する
- そのリポジトリをクローンする

## 3. ローカルでの作業

- あらかじめDocker Desktopを起動しておく
- VSCodeでフォルダ（リポジトリ）を開く
- 開発コンテナを設定する
- 左下にある「大なり小なり」アイコン ＞ 「コンテナで再度開く」実行
- VSCode内で、ターミナルを開く
- `npm create cloudflare@latest`コマンド実行
- cloudflareをインストールするか？ にYesと答える
- サブフォルダ名（＝サーバー名）を決める
- Hello World example > Static site
- wrangler.jsoncファイルに、`{dev: {ip: "0.0.0.0"}}`を追加する（開発コンテナを使っているため）
- `npm run dev`コマンド実行 => ブラウザで`http://localhost:8787`を開く
- publicフォルダの中のhtmlファイルを開発していく

## 4. Cloudflareにデプロイする

- wrangler.jsoncファイルに、`{account_id: "xxxxx"}`を追加する
- .envファイルを作成し、`CLOUDFLARE_API_TOKEN: zzzzzz`を追加する
- `npm run deploy`コマンド実行

## 5. ファビコンを用意する

- スマホで自撮りする
- Geminiでアニメ風にかわいい感じに補正する
- GIMPを使って、丸く切り取る。丸の外側は透明にする
- JPEGは透明をサポートしていないので、PNGで保存する
- ファビコンジェネレーターを使って、ファビコンに変換する
