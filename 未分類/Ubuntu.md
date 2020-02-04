---
tags: ubuntu
----

# Ubuntu

作成日 2019/12/26

## 01. HWEとは

Ubuntu 18.04にログインする度に、以下のメッセージが表示される

`Your Hardware Enablement Stack (HWE) is supported until April 2023.`

これは何を意味しているのか？

[Kernel/LTSEnablementStack \- Ubuntu Wiki](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)

> The Ubuntu LTS enablement (also called HWE or Hardware Enablement) stacks provide newer kernel and X support for existing Ubuntu LTS releases. These enablement stacks can be installed manually but are also available when installing with Ubuntu LTS point release media. 

2023年の4月というのは、Ubuntu 18.04 LTSのサポート期限のことと理解した。\
Ubuntuがハードウェアを調べて、耐用年数を報告しているわけではない

### HWEをインストールする

[Ubuntu18\.04にカーネル 5\.0 をインストールする手順](https://www.virment.com/how-to-update-kernel-5-in-ubuntu/)

```bash=
uname -a
# => Linux amachan1 5.0.0-37-generic #40~18.04.1-Ubuntu SMP Thu Nov 14 12:06:39 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```

すでにカーネルは5.0になっていることを今知った
