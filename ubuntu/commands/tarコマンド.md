# tar コマンド

作成日 2020/02/03

## tar コマンドの使用例

```bash
tar -cvf linux_notes.tar notes*.txt
# オプション (c)reate (v)erbose (f)ile

tar -xvf linux_notes.tar
# オプション e(x)tract (v)erbose (f)ile

# designサーバーの/mnt/efg/goodsディレクトリをバックアップする
cd /mnt/efg
tar -cvf mnt-efg-goods.tar goods/*
ls -l --block-size=M
#=> 59M mnt-efg-goods.tar

# 他のサーバーで/mnt/efg/goodsディレクトリをレストアする
sudo mkdir -p /mnt/efg
cd /mnt/efg
sudo tar -xvf /var/www/copy/mnt-efg-goods.tar
```
