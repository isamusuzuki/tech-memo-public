# ハードウェア情報を調べるコマンド

作成日 2019/11/05

以下のコマンドは、Ubuntu 18.04 で動作を確認した

| Command       | Comment                                   |
| ------------- | ----------------------------------------- |
| lscpu         | CPU 情報を表示する                        |
| lspci         | PCI 情報を表示する                        |
| lsusb         | USB 情報を表示する                        |
| free -m       | 現在のメモリ使用状況を表示する            |
| top           | 現在のメモリ使用状況を全画面表示する      |
| df -h         | ディスク情報を表示する                    |
| lsblk         | list block devices ディスク情報を表示する |
| sudo fdisk -l | ディスク情報を表示する                    |

## du コマンド

ディレクトリのサイズを調べる

```bash
cd ~
du -s -m environment
#=> 50  environment （environmentディレクトリのサイズは50MBという意味）
```

-   `-s` ... サマリーだけ出す
-   `-m` ... 単位を MB にする
