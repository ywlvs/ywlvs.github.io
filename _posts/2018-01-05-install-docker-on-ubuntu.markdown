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

### Prerequisites

Docker 支持 Ubuntu 的以下版本，并且要求系统为 64 位。

- Artful 17.10 (Docker CE 17.11 Edge and higher only)

- Zesty 17.04

- Xenial 16.04 (LTS)

- Trusty 14.04 (LTS)

>另外，要求您的 Ubuntu 基于 `x86_64`, `armhf`, `s390x(IBM Z)` 以及 `ppc64le(IBM Power)` 的架构。

### Uninstall old versions

旧版本的 Docker 被称为 `docker` 或者 `docker-engine`，如果之前有安装，先将它们卸载掉。

```
$ sudo apt-get remove docker docker-engine docker.io
```

如果并没有安装过就版本，并且上述 `apt-get` 命令提示没有安装过上述程序也是 OK 的。

在 `/var/lib/docker` 目录中，依旧保留着镜像（images）、容器（containers)、卷（volumes）和 networks。Docker CE 的 package 现在的名称是 `docker-ce`。

### Install Docker CE

安装 Docker CE 有多种方法，根据实际需求进行选择。

+ 大多数用户先建立 Docker 的仓库（Docker's repositories)，使用仓库可以轻松地进行安装升级等操作（注：官网上推荐本方式）。

+ 还有一种方法是下载 DEB 的安装包，然后进行手动的安装以及升级。这种方式通常用于无法联网的情况（air-gapped systems with no access to the internet）。

+ 在测试和开发环境中，也有用户使用自动脚本的进行安装（automated convenience scripts）。

#### Install using the repository

首次安装时，需要先准备好 Docker 的仓库（Docker repository)，完成该操作之后，就可以通过仓库进行安装和升级等操作。

##### SET UP THE REPOSITORY

1. 更新 `apt` 的 `package` 索引：

```
$ sudo apt-get update
```

2. 安装一些必要的包，从而可以使用 `apt` 命令通过 `HTTPS` 访问仓库。

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


4. 使用下面的命令来建立稳定版的仓库（`stable repository`)。如果想创建一个 `edge` 或者 `test` 版本的 `repository`，可以在下面命令中 `stable` 后面加上你想要的版本，也可以两个都加上。不过，在任何时候，一个稳定版本的仓库都是必备的。

>下面命令中，`lsb_release -cs` 子命令会返回你正在使用的 `Ubuntu` 发行版本的名字，比如 `xenial`。对于某些发行版本，比如 `Linux Mint`，你可能需要使用它上一个 Ubuntu 的发型版本（parent Ubuntu distribution）来代替 `$(lsb_release -cs)` 命令，比如你使用的是 `Linux Mint Rafaela`，你可以将该子命令用 `trusty` 进行替换。

+ x86_64 / amd64

```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

+ armhf

```
$ sudo add-apt-repository \
   "deb [arch=armhf] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

+ IBM Power (ppc64le)

```
$ sudo add-apt-repository \
   "deb [arch=ppc64el] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

+ IBM Z (s390x)

```
$ sudo add-apt-repository \
   "deb [arch=s390x] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

>注：从 Docker 17.06 开始，stable releases 也被加到了 edge 和 test 的仓库。

##### INSTALL DOCKER CE

1. 更新 apt package 的索引

```
$ sudo apt-get update
```

2. 安装最新版本的 Docker CE，或者跳到下一步，安装自己想要的某个版本。

```
$ sudo apt-get install docker-ce
```

>如果设置了多个仓库（Docker repositories），使用 `apt-get install` 或者 `apt-get update` 命令进行 Docker 的安装或者升级时，如果没有指定版本，会默认安装最新的版本，这可能并不适合你想安装稳定版的初衷（may not be appropriate for your stability needs）。

3. 在生产环境下安装 Docker 时，应该指定 Docker CE 的版本，而不是总是使用默认的最新版。使用 `apt-cache madison docker-ce` 命令可以查看当前有哪些可用的版本。

```
$ apt-cache madison docker-ce

docker-ce | 17.12.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
```

>上面的结果被截断了（truncated），实际情况下，可能会输出一个列表，而不仅仅只有一条记录。

上面列表的具体内容取决于当前正在使用的是哪个版本的仓库。从中选择一个版本进行安装。输出记录中，第二列表示版本，第三列是仓库的名字，通过第三列可以看出 packages 是源于哪个仓库，并从此推断出它的稳定性（stability level）。将指定的版本号加到 package name 的后面，并使用等号（=）将它们分开。

```
$ sudo apt-get install docker-ce=<VERSION>
```

Docker 的守护进程（daemon）会自动启动运行。

4. 通过运行 `hello world` 镜像来检查 Docker CE 是否正确安装。

```
$ sudo docker run hello-world
```

该命令会下载一个测试镜像并在容器中运行，当该容器运行时，它会打印一些信息然后退出。

至此，Docker CE 就完成安装并运行了。这时，系统中已经建立了名为 `docker` 的用户组，但是里面并没有用户，这种情况下，运行 Docker 命令需要添加 sudo。

##### UPGRADE DOCKER CE

如果想升级 Docker CE，首先运行 `sudo apt-get update`，然后按照安装说明，选择安装自己想要的版本。
