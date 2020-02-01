# Nvidia オンラインコースを勉強する

作成日 2019/10/13、更新日 2019/10/20

## 0. コース情報

-   Title ... Getting Started with AI on Jetson Nano
-   URL ... [https://courses.nvidia.com/courses/course-v1:DLI+C-RX-02+V1/info](https://courses.nvidia.com/courses/course-v1:DLI+C-RX-02+V1/info)

## 1. Welcome

省略

## 2. Setting up your Jetson Nano

### Introduction

省略

### Prepare For Setup

省略

### Write Image To The MicroSD Card

このページに OS イメージのリンクがある。[Jetson Download Center](https://developer.nvidia.com/embedded/downloads)にある OS イメージとは別物なので要注意

#### Setup And First Boot

初回起動は時間がかかる

| Key | Value   |
| --- | ------- |
| ID  | dlinano |
| PW  | dlinano |

IP アドレスがわかれば、SSH 接続できるようになっている

```bash
ssh dlinano@192.168.1.14
```

#### Camera Setup

-   カメラの帯ケーブルの金属の筋とコネクターの金属の筋を合わせる
-   カメラは、帯ケーブルが上にくるようにセットする（そちらが上）

### Hello Camera

`http://192.168.1.14:8888` で、JupyterLab にもログイン可能

ログインの時のパスワードは`dlinano`

nvdli-nano/hello_camera/csi_camera.ipynb を開く

/dev/video0 があれば、カメラ接続は成功

### JupyterLab

JupyterLab の画面構成

| Key            | Value                              |
| -------------- | ---------------------------------- |
| Menu bar       | 上部にあるメニュ＝                 |
| Left sidebar   | 左枠                               |
| Main Work area | 本枠                               |
| Launcher       | ログイン時に最初に開いているページ |

JupyterLab の基本操作

-   `.ipynb` ... IPython notebook ファイルの拡張子
-   `cells` ... notebook は、テキストセルとコードセルから成り立っている
-   `run` ... コードセルを走らせると、結果が出力される
-   `execution count` ... コードセルの左側には、走らせた回数が表示されている

## 3. Image Classification

### AI and Deep Learning

### Covolutional Neural Networks (CNNs)

### ResNet-18

### Thumbs Project

### Emotions Project

### Assessment
