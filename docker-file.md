[返回首页](README.md)

## 如何理解客户端的导航按钮/界面

> * 最开始，用户希望服务器部署在自己家。而不是打开之后注册登录公共的服务器。
> * 接着，用户把自己的服务器建立起来之后，发现还需要有客户端应用程序才可以。
> * 用户自建app，那么问题到此结束。导航是不需要配置的。
> * 如果用户手里没有app，也就是说没有客户端、用户端应用程序。
> * 接下来，用户发现有一个选择，就是用开源社区提供的app。
> * 紧接着，用户发现需要配置app(应用程序)，使其去连用户自己配置的服务器。
> * 登录页面中的选择导航按钮的作用就是让app去连你自己的服务器（而不是公有服务器）


我们希望我们的平台可以构建在企业环境中，因此我们推荐用户做私有化部署。

但是在私有化部署之前，有必要了解这套系统是怎么工作的，需要哪些服务，都有哪些功能。

所以在部署前进行有效的测试，将会让各位更加明晰当前版本的功能范围和性能指标。

我们推荐的试用分为两个方向：

* 注册公共域试用
* docker部署试用


# 最新docker-compose项目:
https://github.com/startalkIM/startalk-docker-compose

# 以下文档为最初期的startalk-docker部署文档, 已过期  

## 提前准备

* 客户端
    * iOS客户端 (暂缺，审核中)
    * [Android客户端](https://i.startalk.im/home/#/download)
    * [PC客户端 32bit](https://i.startalk.im/pubapi/soft/download/StarTalk_x86.exe)  (推荐)
    * [PC客户端 64bit](https://i.startalk.im/pubapi/soft/download/StarTalk_x64.exe)  (推荐)
    * [Mac客户端](https://i.startalk.im/pubapi/soft/download/StarTalk.dmg)
    * linux 客户端
    * Web 客户端

# Docker部署
Docker起测试服务是我们目前推荐的方式。

Docker 可以在Windows上起服务，这使得很多Windows用户不需要额外准备Linux主机就可以体验到startalk的各方面能力。

## 下载Docker

+ 下载/安装 Docker
    * [Windows版本](https://docs.docker.com/docker-for-windows/install/)
    * [Mac版本](https://docs.docker.com/docker-for-mac/install/)
    * [Ubuntu版本](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
    * [Centos版本](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [Debian版本](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [其他Linux发行版](https://docs.docker.com/install/linux/docker-ce/binaries/)

## 部署Docker

### 安装步骤
+ 获取资源[文件](https://i.startalk.im/pubapi/soft/download/startalk_docker.zip)  并解压:
```
wget https://i.startalk.im/pubapi/soft/download/startalk_docker.zip
```
+ 解压文件
```
unzip -x startalk_docker.zip
```
得到：
```
-rw-r--r--. 1 root root 1.3K 12月 10 2019 Dockerfile
drwxr-xr-x. 2 root root   33 12月 11 2019 images
-rw-------. 1 root root  22M 12月 10 2019 permfile.zip
-rw-r--r--. 1 root root 1.9K 12月 11 2019 README.md
-rw-r--r--. 1 root root  93K 12月 10 2019 success.png

```
之后再解压：
```
unzip -x permfile.zip
```
得到:
```
-rw-r--r--. 1 root root 1.3K 12月 10 2019 Dockerfile
drwxr-xr-x. 2 root root   33 12月 11 2019 images
drwxr-xr-x. 6 root root   57 12月  9 2019 permfile
-rw-------. 1 root root  22M 12月 10 2019 permfile.zip
-rw-r--r--. 1 root root 1.9K 12月 11 2019 README.md
-rw-r--r--. 1 root root  93K 12月 10 2019 success.png
```
+ 用docker加载镜像
```
docker load < images/startalk_docker.tar
```
+ 创建 Volume
> 为什么要创建volume?
> 
> 简单来说，Docker环境在虚拟机下无法保存数据。你需要用你的宿主机来保存这些文件，比如日志，比如图片，比如聊天记录。
```
docker volume create startalkpgdata
```

+ 获取 docker image id
```
docker images
回车
然后，在结果中复制SIZE最大的那一列对应的IMAGE ID
``` 

+ 启动镜像 
> 需要提前准备：
> * 自己宿主机的IP (下文中的hosturl)
> * 上一步获得的docker image id (下文中的镜像id)

* liunx 用户使用：
```
假设:
permfile 全路径为 /home/xxx/permfile
宿主机IP是 192.168.0.1
docker image id 是 1e977139e6ba

那么直接运行：
docker run  -v /home/xxx/permfile:/startalk/permfile -p 8080:8080 -p 5202:5202 -e hosturl="192.168.0.1"  1e977139e6ba
```

**注意，Docker当前版本必须使用全路径加载permfile,否则可能大概率启动失败**

* Windows 用户使用：
```

假设:
宿主机IP是 192.168.0.1
docker image id 是 1e977139e6ba

那么直接运行：
docker run  -v permfile:/startalk/permfile -v startalkpgdata:/startalk/data -p 8080:8080 -p 5202:5202 -e hosturl="192.168.0.1"  1e977139e6ba
```

注意：

 * docker run 加 -v参数，后面接的是持久性文件在宿主机上的绝对路径。
 如您将permfile.zip 解压到了 /home/startalk/permfile 则-v /startalk/permfile:/startalk/permfile.冒号后面的路径不需改动  
 * docker run 加了-p参数，配置了宿主机与container的端口映射。本例中需要用到宿主机暴露两个端口8080 5202
 * hosturl 是宿主机的ip，作为参数传给container。如果您配置的是内网ip 那整套IM 只能在内网使用。否则，如果您配置的是公网ip 则外网也可以使用 
 * docker 版本：build image 是用的 Docker version 18.09.2，避免因为版本问题，建议使用>=18.09


这里是原文 [地址](https://github.com/startalkIM/startalk-docker)

[返回首页](README.md)
