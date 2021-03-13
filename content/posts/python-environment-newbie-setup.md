+++
title = "Docker 建構簡單 Python 開發環境"
date = 2021-03-07T22:01:39+08:00
tags = ["筆記", "python", "docker"]
categories = ["python", "docker"]
draft = false
description = "快速建構隔離可控制的python開發環境"
+++

<!--more-->
---

## 用途
常因python環境及套件問題困擾，在新手上路期間，需要即刻殺敵時非常困擾，為了方便快速達作戰前線，使用Docker快速建構隔離可控制之python環境。

python套件及版本控制相關工具。
* [pyenv](https://github.com/pyenv/pyenv) python版控切換套件
* [pipenv](https://pypi.org/project/pipenv/) python推薦的套件管理工具
* [virtualenv](https://virtualenv.pypa.io/) 建立python虛擬環境，隔離各套件版本
* [conda](https://docs.conda.io/) 多語言的依賴及環境管理工具

## Docker建置

1. Pull `ubuntu18.04`
```bash
docker pull ubuntu:18.04
```

2. 檢查Local Images
```bash
docker images
```

3. 啟動一個Container並使用Bash

```bash
docker run -it ubuntu:18.04 bash
```

4. 安裝一些ubuntu需要的工具

```bash
apt update
apt install iputils-ping wget net-tools vim
```

5. 安裝python

```bash
apt install python3.7
ln -s /usr/bin/python3.7 /usr/bin/python
// ln -s 代表建立軟連結
```

6. 安裝pip
```bash
apt install python3-pip
ln -s /usr/bin/pip3 /usr/bin/pip
// ln -s 代表建立軟連結
```

7. 查看所有Container
```bash
docker ps -a
```

8. 將Container打包成一個Image
```bash
docker commit <Container ID> hao/python3.7-dev
```

9. 查查看Local Images，將會看到`hao/python3.7-dev`
```bash
docker images 
```

10. 直接使用剛剛打包的Image直接啟動新的容器
```bash
docker run -it <Image>
```

* 進入Container使用bash
```bash
docker exec -it <container> bash
```

* 啟動/關閉 Container
```bash
docker start <container>
docker stop <container>
```

* 尋找Linux Command位置
```bash
type -a pip3
```

## 後計
如果熟悉Python開發生態系，使用`conda`或`pipenv`就可以了。

