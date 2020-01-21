# 简介
Startalk 是一套开源IM系统平台。涵盖了所有你所能想到的目标平台和目标系统。且性能优异。

Startalk主攻 toB 场景。

Startalk的目标是想解决企业在近20年的时间内没有统一的IM系统的问题:

```
相比之下，各大操作系统经过近20年的演化，以及nginx的出现，已经让http服务变得非常简单了。
```

简单来说，我们的优点很明显:

```
第一，性能、稳定性以及平台适应性可以算是业界最好的，理论上支持所有平台
第二，公开了几乎全部的代码
第三，团队仍旧在持续迭代
```

缺点也有：

```
1. 系统很庞大和复杂
2. 涉及到的技术栈面很广，通常情况下您需要先想好自己要做什么，想实现什么功能，以及需要哪方面帮助
```


之前写过一篇介绍。本篇希望在叙述上更针对典型用户的需求，如果对上一个版本的介绍文档感兴趣，可以看看[这里](https://github.com/startalkIM/startalk_backup)。

### 本文的目录

* [我们的口号](#口号)
* [系统特性](#系统特性)
    * [安全性](#安全性)
    * [性能特性](#性能特性)
    * [纯native实现，全平台支持](#全平台支持)
    * [支持自建应用嵌入](#自建应用)
* [适用场景](#适用场景)    
* 开始试用(部署基础服务，3个步骤)
    1. 部署后台基础功能(三选一)
        * [通过一键安装包安装后台](#一键包)
        * [通过部署Docker安装后台](docker-file.md)
        * [使用源码通过文档部署](source-build.md)
    1. 生成注册用户
        * [生成用户](create-user.md)
    1. 部署客户端 
        * [下载客户端](config-client.md)
        * [配置客户端](config-client.md)
* 部署扩展功能
    1. [部署Push服务（推送服务）](https://github.com/startalkIM/push_service)
    1. [部署音视频](https://github.com/startalkIM/call_room_server)
    1. [部署红包、AA]()
* 其他需求
    * [二次开发](sdk-introduction.md)
    * [商业客服 Saas]()
* [系统架构](#系统架构)
    * [功能组件列表](#功能组件列表)
    * [后端](#后端)
    * [PC端](#PC端)
    * [iOS端](#iOS端)
    * [android端](#android端)
* [常见问题](#常见问题)    
* [问题反馈](#问题反馈)
* [相关资源入口](#相关资源入口)

## 为什么要用IM
```
人类从有沟通需求开始，追求无止境。
```
沟通即表达，传递信息和想法

# 口号

我们的口号是
```
不做里程碑，也要做铺路石!
```

# 系统特性

IM系统本身很复杂。投入产出比不高。常见的开源方案的目标并不是性能和稳定性，大多以实现功能为主。

星语Startalk主要针对底层实现进行了较长时间的积累和沉淀。 这个也契合了[我们的口号](#口号)

谈到系统特性，主要围绕以下几个方面展开：

* [安全性](#安全性)
* [性能](#性能/扩展性)
* [纯native实现，全平台支持](#全平台支持)
* [支持自建应用嵌入](#自建应用)


# 安全性
* 自有协议
```
    不同于常见的im协议，startalk的协议是自有协议。
    由于是自有协议，增大了传输过程中被窃听的复杂度和难度。
```
* 二进制协议
```
    区别于常见的文本协议（例如xmpp，又或者是http），startalk的通信协议是二进制的。
    这个设计也增大了传输过程中被窃听的复杂度和难度。
```
* TCP长连接，默认全程TLS，保持端到端加密
```
    由于连接开启了TLS，进一步增加了传输过程中被窃听的难度。
```
* 服务端在自己机房，客户端在自己用户手中
```
    startalk作为开源软件,区别于常见的通信软件服务,推荐用户进行私有化部署。
    当用户私有化部署完成之后,用户间的通信过程就像公司内部的邮件和bbs一样，只于内部系统可见了。
```

# 性能特性

* 轻松支持百万级用户在线，支持横向扩展
```
    因为支持几亿人同时在线的IM已经做的太强大了。
    所以，startalk的目标和典型场景，是支持一个正常团队的人数协同工作。
    这个数量从十几、几十到上百上千甚至上万。
    startalk非常善于轻松支持这个人数的用户进行协作。
    
    当然，由于Startalk的后端基于erlang。这使得横向扩展变得非常容易。
    通常我们需要支持上百万用户的时候，
    除了加内存之外，还可以选择买机器，然后用集群来解决问题。

    一切都是非常的简单和自然。
```

* 客户端更省电，最高可达90%
```
    我们测试了常见的xmpp服务端。
    由于startalk用了二进制协议和自有协议，这使得协议在解析过程中更加的节省CPU，
    从而节约了最高90%的电能损耗。
    目前startalk的耗电量跟微信在同一水平线上(比常见的xmpp客户端省电90%)。

```

* 客户端更省流量，最高可达90%
```
    相信很多使用xmpp客户端的用户都有同感，每天没怎么聊流量用的很多。

    同样由于优化了协议和传输，使得startalk比常见的xmpp客户端最高可节约90%流量。
    
    同样的，目前startalk的流量损耗跟微信也在同一水平线上(比常见的xmpp客户端节约90%流量)。
```

* 不丢消息
```
    看起来是个很简单、很容易做到的功能，
    然而对聊天软件来说，丢消息、消息不实时是致命的。
    可怕的是，这个功能对很多产品来说却很难做好。
    startalk经过了近3年的高强度测试和改进，终于做到了这个目标。
```

* 消息量大了不卡顿
```
    同丢消息一样，这个点也是看起来很简单，很容易做到的功能，
    然而确是大量软件不太容易做到的功能。

    这个问题的现象是，起初刚开始用的时候很好用。等消息多起来之后，
    列表页卡顿，消息页卡顿，优化也很耗精力和时间。
    星语可以做到10G左右的聊天数据不卡顿。(微信还要更猛一些)
    使用星语做您软件的底层可以尽可能的避免遇到这些问题。
```

# 全平台支持

与市面上主流面向web的通信服务工具不同，Startalk 面向平台，为了实现更强大的性能上限，客户端用了本地化的代码实现。


Startalk 由以下客户端组成：

* [iOS客户端，基于Objective-c](https://github.com/startalkIM/imsdk-ios)
* [Android客户端，基于Java](https://github.com/startalkIM/imsdk-android)
* [Web客户端, 基于JS](https://github.com/startalkIM/startalk_web)
* [PC端, 基于c++](https://github.com/startalkIM/startalk_pc)

# 自建应用

(建设中)

# 适用场景

* 企业办公OA
* 企业应用整合
* SDK嵌入(企业自有业务支撑)
* 商业客服

# 企业办公OA

* [通信]
* [邮件]
* [审批]
* [APP整合]

# 企业应用整合

* [客户端SDK]
* [OA二次开发]

# SDK嵌入

#### **IOS sdk  嵌入**：
###### Cocoapods集成
###### QIMSDK最低支持iOS9系统

###### 默认依赖React-Native的CocoaPods集成

1. 在Podfile中加入以下内容：
 
	```pod
        source 'https://github.com/startalkIM/libqimkit-ios-cook.git'

        source 'git@github.com:CocoaPods/Specs.git'
		
    	target 'YourApp' do
    	
	      pod 'QIMUIKit', '~> 4.0'

        end
 
        post_install do |installer_representation|
 
        installer_representation.pods_project.targets.each do |target|
         target.build_configurations.each do |config|
             config.build_settings['APPLICATION_EXTENSION_API_ONLY'] = 'NO'
         end
       end
     end
    ```
 
2. 在项目根目录执行 `pod install`


#### **Android sdk  嵌入**：

###### Configure Gradle

```gradle
buildscript {
    repositories {
        google()
        jcenter()
    }
}
 
allprojects {
    repositories {
             maven { url 'https://dl.google.com/dl/android/maven2' }
             maven {
                 url "https://raw.githubusercontent.com/froyomu/im/master"
             }
             maven {
                 url "http://developer.huawei.com/repo/"
             }
             jcenter {
                 url "http://jcenter.bintray.com/"
             }
             maven {
                 url "https://jitpack.io"
             }
             google()
             jcenter()
             mavenCentral()
        }
}
```

```gradle
dependencies {
  compile project(':imsdk')//compile 'com.qunar.im:sdk-im:3.0.7'
}
```
###### Deploy manifestPlaceholders

```manifestPlaceholders
flavorDimensions "qim"
    //Mutiple channels
    productFlavors {
        //startalk
        startalk {
            dimension "qim"

            manifestPlaceholders = [
                    PACKAGE_NAME : "sdk.im.qunar.com.qtalksdkdemo",//Replace it with the application ID of your own project
                    serverDoMain  : true,
                    baiduMap :"xxxxx",//key of Baidu map (for sending location)
                    HUAWEI_APPID : "123",//HUAWEI push
                    OPPO_APP_ID : "123",//OPPO push
                    OPPO_APP_KEY : "123",
                    OPPO_APP_SECRET : "123",
                    MIPUSH_APP_ID : "123",//XIAOMI push
                    MIPUSH_APP_KEY : "123",
                    MEIZU_APP_ID : "123",//MEIZU push
                    MEIZU_APP_KEY : "123",
                    VIVO_APP_ID : "123",//vivoo
                    VIVO_APP_KEY : "123",
                    SCHEME : "qtalkaphone",
                    currentPlat  : "QTalk",
                    MAIN_SCHEMA : "start_qtalk_activity"
            ]
        }
    }
```
###### Configure Manifest of main project

```
Please see the AndroidManifest configuration in app
```
  
* [Web嵌入]


# 商业客服


# 系统架构

## 后端

### 2.0 版本架构图

(暂缺)

# 功能组件列表

详细和原始的内容请参考[这里](https://github.com/startalkIM/ejabberd/blob/master/README.md#startalk-%E6%A8%A1%E5%9D%97)

+ [ejabberd](https://github.com/startalkIM/ejabberd) (erlang)

IM核心组件，负责维持与客户端的长连接和消息路由

+ [openresty 组件](https://github.com/startalkIM/openresty_ng)(LUA)

IM负载均衡组件，负责验证客户端身份，以及转发http请求到对应的后台服务

+ [im_http_service](https://github.com/startalkIM/im_http_service) (JAVA服务)

IM HTTP接口服务，负责IM相关数据的查询、设置以及历史消息同步(基于tomcat的java服务)

+ [qfproxy](https://github.com/startalkIM/qfproxy)(JAVA服务)

IM文件服务，负责文件的上传和下载(基于tomcat的java服务)(JAVA服务)

+ [push_service](https://github.com/startalkIM/push_service)(JAVA服务)

IM的push服务，用于离线消息的推送(基于tomcat的java服务)(JAVA服务)

+ [search_serivce](https://github.com/startalkIM/search)(JAVA服务)

提供远程搜索好友、群、聊天记录的服务

+ redis

IM缓存服务

+ postgresql

 数据持久化服务   


 ## PC端

 (建设中)

技术文档可以先参阅[这里](https://github.com/startalkIM/startalk_pc)



 ## iOS端

 (建设中)
 技术文档可以先参阅[这里](https://github.com/startalkIM/imsdk-ios)

## Android端

(建设中)
技术文档可以先参阅[这里](https://github.com/startalkIM/imsdk-android)


# 试用


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

## 提前准备


# 一键包


[https://i.startalk.im/home/#/download_easy](https://i.startalk.im/home/#/download_easy)


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
docker run  -v /home/xxx/permfile:/startalk/permfile -v startalkpgdata:/startalk/data -p 8080:8080 -p 5202:5202 -e hosturl="192.168.0.1"  1e977139e6ba
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

# 公共域注册登录

起先我们是推荐在[线上](https://www.startalk.im/)注册登录试用的。因为设备费用等问题以及人手的问题，
我们慢慢不太建议少量用户试用公共域了（当然您还可以继续使用它）。

如果您目前处于:
 * 需要一款完全私有化部署的系统
 * 暂时没精力折腾
 * 能接受暂时的公有域使用，当自己条件满足之后切成私有部署

那么您可以尝试暂时在公共域[入驻](https://i.startalk.im/home/#/register)了！


# 获取客户端
* 请在[这里](https://i.startalk.im/home/#/download)下载各平台最新版本客户端

[[返回目录列表](#简介)]


# 生成用户

这个部分是后来写的。如有需要，请参见 [原始文档](https://github.com/startalkIM/ejabberd/wiki/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)

当前startalk生成用户的部分主要是通过执行sql，在后台生成，目的是扩展性。

需要手动生成用户，首先要明确几个问题：


+ startalk后台使用秘文密码。密码规则: 参照[密码规则](https://github.com/startalkIM/ejabberd/wiki/%E5%AF%86%E7%A0%81%E7%94%9F%E6%88%90%E8%A7%84%E5%88%99)

+ 用户id (大部分文档中会称之为user_id)生成规则：只能包含小写字母、数字、_、-、.，建议使用用户中文名的拼音当作user_id。

+ 为了让变更生效，在变更完信息后，需要：
    * host_users需要把version变成max(version) + 1
    * vcard_version需要把version=version + 1


插入用户 (使这个人存在)
```
insert into host_users (host_id, user_id, user_name, department, dep1, pinyin, frozen_flag, version, user_type, hire_flag, gender, password, initialpwd, pwd_salt, ps_deptid) values ('1', 'file-transfer', '文件传输助手', '/智能服务助手', '智能服务助手', 'file-transfer', '1', '1', 'U', '1', '1', 'CRY:fd540f073cc09aa98220bbb234153bd5', '1', 'qtalkadmin_pwd_salt_d2bf42081aab47f4ac00697d7dd32993', 'qtalk');
```

插入名片 (变更这个人的信息，例如名称，头像，等)
```
insert into vcard_version (username, version, profile_version, gender, host, url) values ('file-transfer', '1', '1', '1', 'qtalk', '/file/v2/download/avatar/new/daa8a007ae74eb307856a175a392b5e1.png?name=daa8a007ae74eb307856a175a392b5e1.png&file=file/daa8a007ae74eb307856a175a392b5e1.png&fileName=file/daa8a007ae74eb307856a175a392b5e1.png');
```
数据字典(db schema)

```
host_users:
序号	字段名称	        字段描述	                        字段类型	
1	host_id	        域信息id	                        bigint
2	user_id	        用户名	                        text
3	user_name	中文名	                        text
4	department	组织架构	                        text
5	tel	        电话	                        text
6	email	        邮箱	                        text
7	dep1	        一级部门                         text
8	dep2	        二级部门                         text
9	dep3	        三级部门                         text
10	dep4	        四级部门	                        text
11	dep5	        五级部门	                        text
12	pinyin	        拼音	                        text
13	frozen_flag	禁止标志	                        smallint
14	version	        版本号	                        integer
15	user_type	用户类型（U）	                character
16	hire_flag	在职标志	                        smallint
17	gender	        性别,1（男）2（女）0（未知）       smallint
18	password	密码	                        text
19	initialpwd	是否是初始密码，1（是）0（否）	smallint
20	ps_deptid	根级组织名字	                text
21      pwd_salt        密码盐值                         character varying(200)
```


## 常见问题



# 相关资源入口

