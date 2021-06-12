# SSH ポートフォワーディング

作成日 2021/03/24

## 01. SSH Port Forwarding とは

紹介記事を読む

[Intel NUC で自宅 Ubuntu 開発環境構築と SSH Port Forwarding によるアクセス \| blog\.jxck\.io](https://blog.jxck.io/entries/2019-11-03/nuc-dev-server-port-forwarding.html)

> 自宅内に置いているため、固定 IP などはない。\
> しかし、せっかく作った環境は、外出先等でも使いたいため、外からもアクセスできるようにしたい。\
> すでに Sakura VPS には固定 IP を振っているため、これを用いた最も安価で簡単な方法は SSH の Port fowarding だろう。\
> NUC と VPS の SSH を張りっぱなしにしておき、laptop からの SSH をそこを通して NUC に届けるのが Port Fowarding だ。

```bash
# nuc から vps
ssh user@vps -NR 22222:localhost:22
# => 繋ぎっぱなしにする

# laptop から vps
ssh user@vps

# vps に入ったあと vps から nuc
ssh user@localhost -p 22222
```
