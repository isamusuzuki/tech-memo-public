# Homebrew

作成日 2025/12/03

## 1. 公式サイト（英語）を読む

[Homebrew — The Missing Package Manager for macOS (or Linux)](https://brew.sh/)

インストール

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

最新バージョンがいくつなのかは、GitHubのページでわかる

[Homebrew/brew: 🍺 The missing package manager for macOS (or Linux)](https://github.com/Homebrew/brew)

## 2. brewコマンド集

```bash
# ヘルプを表示する
brew

# パッケージを検索する
brew search {keyword}

# パッケージの詳細を表示する
brew info {package-name}

# パッケージをインストールする
brew install {package-name}

# インストール済みパッケージを表示する
brew list

# パッケージをアンインストールする
brew uninstall {package-name}

# バージョンを表示する
brew --version

# brew本体を更新する
brew update

# インストール済みパッケージを更新する
brew upgrade

# 更新後の不要ファイル削除
brew cleanup
```
