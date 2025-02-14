# Compute Engine で作成した VM インスタンスに SSH 接続する

作成日 2020/02/21

## 01. アクセス方法の選択

[アクセス方法の選択  \|  Compute Engine ドキュメント  \|  Google Cloud](https://cloud.google.com/compute/docs/instances/access-overview?hl=ja)

> Google Cloud で Linux VM インスタンスを実行している場合、\
> そのインスタンスへのユーザーやアプリのアクセスを共有\
> または制限する必要があることがあります。
>
> Linux VM インスタンスへのユーザー アクセスを管理する必要がある場合は、\
> 次のいずれかの方法を使用します。
>
> - OS ログイン
> - メタデータ内の SSH 認証鍵の管理
> - ユーザーにインスタンスへのアクセスを一時的に許可する

## 02. メタデータ内の SSH 認証鍵の管理

[メタデータ内の SSH 認証鍵の管理  \|  Compute Engine ドキュメント  \|  Google Cloud](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys?hl=ja)

> SSH 認証鍵を作成して管理することで、ユーザーはサードパーティ ツールから \
> Linux インスタンスにアクセスできるようになります。
>
> ユーザーが Google Cloud プロジェクトのメンバーではなくても、秘密 SSH 認証鍵を提示すれば、\
> サードパーティ ツールを使用して、対応する公開 SSH 認証鍵ファイルで構成されたインスタンスに接続できます。

### 新しい SSH 認証鍵を作成する

Git Bash を開く

```bash
ssh-keygen -t rsa -f ~/.ssh/avocado -C gcp-user
mv ~/.ssh/avocado ~/.ssh/avocado.pem
```

### プロジェクト全体に公開 SSH 認証鍵を追加する

GCP コンソール ＞ Compute Engine ＞ 左枠 ＞ メタデータ ＞ 右枠 ＞ SSH 認証鍵\
編集ボタン ＞ 項目を追加ボタン ＞ `~/.ssh/avocado.pub`ファイルをコピペする　＞ 保存ボタン\
コピーが終わったら、`~/.ssh/avocado.pub` は捨てちゃう\
残った avocado.pem を厳重に保管する\
AWS と合わせることができた！

### 接続を確認する

Git Bash を開く

```bash
ssh -i ~/.ssh/avocado.pem gcp-user@100.100.100.100
```

=> 成功！
