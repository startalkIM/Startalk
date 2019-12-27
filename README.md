## 简介
startalk 是一套开源界IM系统平台。涵盖了所有你所能想到的目标平台和目标系统。且性能优异。

startalk主攻 toB 场景。Startalk的目标是想解决企业在近20年的时间内没有统一的IM系统的问题。

```
相比之下，各大操作系统经过近20年的演化，以及nginx的出现，已经让http服务变得非常简单了。
```

[这里](https://github.com/startalkIM/startalk_backup) 是之前的文档，可以看看之前的介绍。

本文将分几个部分介绍这套系统，以及系统能给用户带来的价值。

* [适用场景](#适用场景)
* [试用](#试用)
* [系统架构](#系统架构)
* [问题反馈](#问题反馈)
* [相关资源入口](#相关资源入口)

### 为什么要用IM
```
人类从有沟通需求开始，追求无止境。
```
沟通即表达，传递信息和想法


## 适用场景

* 企业办公OA
* 企业应用整合
* SDK嵌入(企业自有业务支撑)
* 商业客服

## 系统架构

## 试用

我们推荐大家做私有化部署。

但是在私有化部署之前，有必要了解这套系统是怎么工作的，需要哪些服务，都有哪些功能。

所以在部署前进行有效的测试，将会让各位更加明晰当前版本的功能范围和性能指标。

我们推荐的试用分为两个方向：

* 公共域注册登录
* docker部署


#### 提前准备

* 客户端
    * iOS客户端 (暂缺，审核中)
    * [Android客户端](https://i.startalk.im/home/#/download)
    * [PC客户端 32bit](https://i.startalk.im/pubapi/soft/download/StarTalk_x86.exe)  (推荐)
    * [PC客户端 64bit](https://i.startalk.im/pubapi/soft/download/StarTalk_x64.exe)  (推荐)
    * [Mac客户端](https://i.startalk.im/pubapi/soft/download/StarTalk.dmg)
    * linux 客户端
    * Web 客户端
* 后端
    * 下载/安装 Docker

### 公共域注册登录

起先我们是推荐在[线上](https://www.startalk.im/)注册登录试用的。因为设备费用等问题以及人手的问题，
我们慢慢不太建议少量用户试用公共域了（当然您还可以继续使用它）。

如果您目前处于:
 * 需要一款完全私有化部署的系统
 * 暂时没精力折腾
 * 能接受暂时的公有域使用，当自己条件满足之后切成私有部署

那么您可以尝试暂时在公共域[入驻](https://i.startalk.im/home/#/register)了！

### docker部署
这是我们目前推荐的方式。

#### 下载Docker
docker 可以在Windows上起服务，这使得很多用户可以直接在windows上尝试到startalk的功能。

这里是[地址](https://github.com/startalkIM/startalk-docker)

## 问题反馈

## 相关资源入口

