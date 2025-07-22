---
title: Docker
date: 2025-04-13 14:51:06
tags:
  - Docker
  - Containerization
  - DevOps
  - Deployment
  - Backend
---

**Docker**

**环境配置难题**

软件开发的一大难题就是环境配置，因为不同用户的计算机环境都不相同，在自己电脑能运行的软件不一定能在其他机器上跑起来。

软件能够正常运行需要保证1.操作系统的设置 2.各种库和组件的安装。例如安装一个Phthon应用，计算机必须要用Python引擎，还必须有各种依赖，可能还要配置环境变量。某些老旧的模块与当前的环境不相容，换一次机器可能就需要重新配置一次环境，旷日费时。解决问题的方法就是带环境安装软件，也就是说安装软件的时候把原始环境一模一样地复制过来。

**虚拟机virtual machine**

虚拟机就是带环境安装的一种解决方法。它可以在一种操作系统里面运行另一种操作系统，例如在Windows系统里运行Linux系统。对于应用程序，虚拟机和真实系统一模一样，对于底层系统，虚拟机就是一个普通文件，不需要可以直接删去。但是，通过虚拟机来还原软件原始环境有几个缺点：

资源占用多 虚拟机会独占一部分内存和硬盘空间

冗余步骤多 虚拟机是完整的操作系统，一些系统级别的操作系统例如用户登录往往无法跳过

启动慢

**Linux容器**

Linux容器（Linux Containers）是Linux发展出的另一种虚拟化技术。

**Linux容器不是一个完整的操作系统，而是对进程进行隔离。**换句话讲，是在正常进程的外面套了一个保护层。对里面的进程来讲，它接触的各种资源都是虚拟的，从而实现与底层系统的隔离。

它相比于虚拟机的优势在于：

启动快 容器里面的应用就是底层操作系统里的一个进程而不是虚拟机内部的进程

占用资源少 容器只占用需要的资源

体积小 容器只要包含用到的组件即可

比较起来，容器类似于轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。

**Docker**

Docker是属于Linux容器的一种封装，提供简单易用的容器使用接口。

Docker将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件会生成一个虚拟容器。程序就在这个虚拟容器里运行，就好像在真实物理机上运行。

Docker目前主要有三个用途：

提供一次性的环境

提供弹性的云服务

组建微服务架构

**Docker安装**

Mac地址：[Docker CE安装官方文档](https://docs.docker.com/desktop/setup/install/mac-install/)

Windows地址：[Docker CE安装官方文档](https://docs.docker.com/desktop/setup/install/windows-install/)

安装过后运行下面的指令

```Plain Text  
$ docker version  
\# 或者  
$ docker info
```
Docker需要用户具有sudo权限，可以把用户加入Docker用户组

```Plain Text  
$ sudo usermod -aG docker $USER
```

Docker是服务器-客户端架构，命令行运行Docker命令的时候，需要本机有Docker服务。启动服务可以用以下命令：

```Bash  
$ sudo service docker start  
//service命令用法  
<br/>$ sudo systemctl start docker  
//systemctl命令用法  
<br/>
```

**Image文件**

**Docker把应用程序及其依赖，打包在image文件里面。**

只有通过这个文件，才能生成Docker容器。image文件可以看作是容器的模版，Docker根据image文件生成容器的实例，同一个image文件可以生成多个同事运行的容器实例。

image是二进制文件。实际开发中，一个image文件往往通过继承另一个image文件加上一些个性化设置而生成。

```Bash  
$ docker image ls  
//列出本机的所有image文件  
<br/>$ docker image rm \[imageName\]  
//删除image文件
```

image文件是通用的，一台机器的image文件可以拷贝到另一台机器，为了节省时间，应尽量使用别人制作好的image文件，即使要定制，也应该基于别人的image文件进行加工而不是从零开始制作。

image文件制作完成后可以上传到网上的仓库，Docker官方仓库链接为：[Docker Hub](https://hub.docker.com/)

**实例**

用最简单的image文件"Hello World"为例

```Bash  
$ docker image pull library/hello-world
```

docker image pull用于抓取image文件，library/hello-world是image文件在仓库的位置

```Bash  
$ docker image ls
```

抓取成功后就可以在本机看到这个image文件了

```Bash  
$ docker container run hello-world
```

运行这个image文件

docker container run命令会从 image 文件，生成一个正在运行的容器实例。

注意，docker container run命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的docker image pull命令并不是必需的步骤。

```Bash  
$ docker container run hello-world  
<br/>Hello from Docker!  
This message shows that your installation appears to be working correctly.... ...
```

有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。

```Bash  
$ docker container run -it ubuntu bash
```

对于那些不会自动终止的容器，必须使用[docker container kill](https://docs.docker.com/engine/reference/commandline/container_kill/) 命令手动终止。

```Bash  
$ docker container kill \[containID\]
```

**容器文件**

**image文件生成的容器实例，本身也是一个文件，称为容器文件**

一旦容器生成，就会同时存在两个文件：image文件和容器文件。而且关闭容器不会删除容器文件，只是容器暂停运行而已。

```Bash  
$ docker container ls  
\# 列出本机正在运行的容器  
<br/>$ docker container ls --all  
\# 列出本机所有容器，包括终止运动的容器
```

上面命令的执行结果中包含容器ID，终止容器运行的docker container kill就需要提供这个ID。

终止运行的容器文件依然会占据硬盘空间

```Bash  
$ dicker container rm \[containerID\]
```