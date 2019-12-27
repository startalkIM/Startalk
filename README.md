# 简介
Startalk 是一套开源IM系统平台。涵盖了所有你所能想到的目标平台和目标系统。且性能优异。

Startalk主攻 toB 场景。

Startalk的目标是想解决企业在近20年的时间内没有统一的IM系统的问题。

```
相比之下，各大操作系统经过近20年的演化，以及nginx的出现，已经让http服务变得非常简单了。
```

之前写过一篇介绍。本篇希望在叙述上更针对典型用户的需求，如果对上一个版本的介绍文档感兴趣，可以看看[这里](https://github.com/startalkIM/startalk_backup) 。

本文将分几个部分介绍这套系统，以及系统能给用户带来的价值。

* [适用场景](#适用场景)
* [试用](#试用)
    * [生成用户](#生成用户)
* [系统架构](#系统架构)
    * [后端](#后端)
    * [PC端](#PC端)
    * [iOS端](#iOS端)
* [常见问题](#常见问题)    
* [问题反馈](#问题反馈)
* [相关资源入口](#相关资源入口)

## 为什么要用IM
```
人类从有沟通需求开始，追求无止境。
```
沟通即表达，传递信息和想法


# 适用场景

* 企业办公OA
* 企业应用整合
* SDK嵌入(企业自有业务支撑)
* 商业客服

# 系统架构

## 后端

### 2.0 版本架构图

(暂缺)

### 部件列表(详细和原始的内容请参考[这里](https://github.com/startalkIM/ejabberd/blob/master/README.md#startalk-%E6%A8%A1%E5%9D%97)):

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

 ## iOS端

 (建设中)

## Android端

(建设中)



# 试用

我们希望我们的平台可以构建在企业环境中，因此我们推荐用户做私有化部署。

但是在私有化部署之前，有必要了解这套系统是怎么工作的，需要哪些服务，都有哪些功能。

所以在部署前进行有效的测试，将会让各位更加明晰当前版本的功能范围和性能指标。

我们推荐的试用分为两个方向：

* 注册公共域试用
* docker部署试用

## 如何理解客户端的导航按钮/界面

> * 最开始，用户希望服务器部署在自己家。而不是打开之后注册登录公共的服务器。
> * 接着，用户把自己的服务器建立起来之后，发现还需要有客户端应用程序才可以。
> * 用户自建app，那么问题到此结束。导航是不需要配置的。
> * 如果用户手里没有app，也就是说没有客户端、用户端应用程序。
> * 接下来，用户发现有一个选择，就是用开源社区提供的app。
> * 紧接着，用户发现需要配置app(应用程序)，使其去连用户自己配置的服务器。
> * 登录页面中的选择导航按钮的作用就是让app去连你自己的服务器（而不是公有服务器）

## 提前准备

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
        * [Windows版本](https://docs.docker.com/docker-for-windows/install/)
        * [Mac版本](https://docs.docker.com/docker-for-mac/install/)
        * [Ubuntu版本](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
        * [Centos版本](https://docs.docker.com/install/linux/docker-ce/centos/)
        * [Debian版本](https://docs.docker.com/install/linux/docker-ce/debian/)
        * [其他Linux发行版](https://docs.docker.com/install/linux/docker-ce/binaries/)


# 公共域注册登录

起先我们是推荐在[线上](https://www.startalk.im/)注册登录试用的。因为设备费用等问题以及人手的问题，
我们慢慢不太建议少量用户试用公共域了（当然您还可以继续使用它）。

如果您目前处于:
 * 需要一款完全私有化部署的系统
 * 暂时没精力折腾
 * 能接受暂时的公有域使用，当自己条件满足之后切成私有部署

那么您可以尝试暂时在公共域[入驻](https://i.startalk.im/home/#/register)了！

# docker部署
这是我们目前推荐的方式。

## 下载Docker
docker 可以在Windows上起服务，这使得很多用户可以直接在windows上尝试到startalk的功能。

这里是[地址](https://github.com/startalkIM/startalk-docker)


# 生成/变更用户

[原始文档](https://github.com/startalkIM/ejabberd/wiki/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)

+ 密码规则: 参照[密码规则](https://github.com/startalkIM/ejabberd/wiki/%E5%AF%86%E7%A0%81%E7%94%9F%E6%88%90%E8%A7%84%E5%88%99)

+ user_id规则：只能包含小写字母、数字、_、-、.，建议使用用户中文名的拼音当作user_id。

+ 每次变更完信息之后，需要：
    * host_users需要把version变成max(version) + 1
    * vcard_version需要把version=version + 1

```
插入用户
insert into host_users (host_id, user_id, user_name, department, dep1, pinyin, frozen_flag, version, user_type, hire_flag, gender, password, initialpwd, pwd_salt, ps_deptid) values ('1', 'file-transfer', '文件传输助手', '/智能服务助手', '智能服务助手', 'file-transfer', '1', '1', 'U', '1', '1', 'CRY:fd540f073cc09aa98220bbb234153bd5', '1', 'qtalkadmin_pwd_salt_d2bf42081aab47f4ac00697d7dd32993', 'qtalk');

插入名片
insert into vcard_version (username, version, profile_version, gender, host, url) values ('file-transfer', '1', '1', '1', 'qtalk', '/file/v2/download/avatar/new/daa8a007ae74eb307856a175a392b5e1.png?name=daa8a007ae74eb307856a175a392b5e1.png&file=file/daa8a007ae74eb307856a175a392b5e1.png&fileName=file/daa8a007ae74eb307856a175a392b5e1.png');
```
数据字典

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

