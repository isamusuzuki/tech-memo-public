# git pull したらエラーが発生

作成日 2023/03/24

GCP 上の VM インスタンスで git pull したら、以下のようなメッセージが登場した

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:uNiVztksCsDhcc0u9e8BujQXVUpKZIDTMczCvj3tD2s.
Please contact your system administrator.
```

グーグル検索してみたら、公式ブログの記事があった

[We updated our RSA SSH host key | The GitHub Blog](https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/)

## 対処法

古いキーを削除して、新しいキーを書き込む

```bash
ssh-keygen -R github.com

sudo apt install jq

curl -L https://api.github.com/meta | jq -r '.ssh_keys | .[]' | sed -e 's/^/github.com /' >> ~/.ssh/known_hosts
```
