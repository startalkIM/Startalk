
[返回首页](README.md#本文的目录)

# 简介

这个部分是后来写的。如有需要，请参见 [原始文档](https://github.com/startalkIM/ejabberd/wiki/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)

当前startalk主要通过执行sql在后台生成用户，目的是扩展性。


# 注意事项
需要手动生成用户，首先要了解几个事情：


+ startalk后台使用秘文密码。密码规则: 参照[密码规则](https://github.com/startalkIM/ejabberd/wiki/%E5%AF%86%E7%A0%81%E7%94%9F%E6%88%90%E8%A7%84%E5%88%99)

+ 用户id (大部分文档中会称之为user_id)生成规则：只能包含小写字母、数字、_、-、.，建议使用用户中文名的拼音当作user_id。

+ 为了让变更生效，在变更完信息后，需要：
    * host_users需要把version变成max(version) + 1
    * vcard_version需要把version=version + 1


# 操作方法

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

[返回首页](README.md#本文的目录)