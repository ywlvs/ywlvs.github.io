---
layout:     post
title:      "在 Ubuntu 中安装 Docker CE"
subtitle:   "Install Docker on Ubuntu 16.04"
date:       2018-01-05 21:30:00
author:     "ywlvs"
header-img: "img/post-bg-docker-logo-whale.png"
catalog: true
tags:
    - Linux，Docker
---

#### Prerequisites

Docker 支持 Ubuntu 的以下版本，并且要求系统为 64 位。

- Artful 17.10 (Docker CE 17.11 Edge and higher only)

- Zesty 17.04

- Xenial 16.04 (LTS)

- Trusty 14.04 (LTS)

#### Uninstall old versions

旧版本的 Docker 被称为 `docker` 或者 `docker-engine`，如果之前有安装，先将它们卸载掉。

```
$ sudo apt-get remove docker docker-engine docker.io
```

如果并没有安装过就版本，并且上述 `apt-get` 命令提示没有安装过上述程序也是 OK 的。

在 `/var/lib/docker` 目录中，依旧保留着镜像（images）、容器（containers)、卷（volumes）和 networks。Docker CE 的 package 现在的名称是 `docker-ce`。

#### Install Docker CE

安装 Docker CE 有多种方法，根据实际需求进行选择。

+ 大多数用户先建立 Docker 的仓库（Docker's repositories)，使用仓库可以轻松地进行安装升级等操作（注：官网上推荐本方式）。

+ 还有一种方法是下载 DEB 的安装包，然后进行手动的安装以及升级。这种方式通常用于无法联网的情况（air-gapped systems with no access to the internet）。

+ 在测试和开发环境中，也有用户使用自动脚本的进行安装（automated convenience scripts）。

#### Install using the repository

首次安装时，需要先准备好 Docker 的仓库（Docker repository)，完成该操作之后，就可以通过仓库进行安装和升级等操作。

1. 更新 `apt` 的 `package` 索引：

```
$ sudo apt-get update
```

2. 安装一些包，从而允许 `apt` 命令通过 `HTTPS` 来访问仓库。

```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

3. 添加 Docker 的官方 `GPG key`

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

使用指纹码 `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88` 来验证该 key 的有效性。通过指纹码的后 8 位 进行搜索验证。

```
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

4. 使用下面的命令来安装 `stable` repository。
