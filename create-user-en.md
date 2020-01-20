
[Content](README-en.md#Contents)

# Introduction

This is the lastest version of document.You can see the original version [here](https://github.com/startalkIM/ejabberd/wiki/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)


we can only register users through sql now. done more like this is to consider the suitability of your project.

# Cautions
+ startalk uses cipher text password in back-end. this is the [rules](https://github.com/startalkIM/ejabberd/wiki/%E5%AF%86%E7%A0%81%E7%94%9F%E6%88%90%E8%A7%84%E5%88%99)

* User Id (signed it as user_id in this document) generation rules are:
it can only contain lowcase letters, number, dash, underline and point.

* you need to do the following after your information updating to take effect:
    * update host_users set version = version + 1 where user_id = xxx
    * update vcard_version set version = version + 1 where user_id = xxx


# detailed steps

## insert a user
```
insert into host_users (host_id, user_id, user_name, department, dep1, pinyin, frozen_flag, version, user_type, hire_flag, gender, password, initialpwd, pwd_salt, ps_deptid) values ('1', 'file-transfer', '文件传输助手', '/智能服务助手', '智能服务助手', 'file-transfer', '1', '1', 'U', '1', '1', 'CRY:fd540f073cc09aa98220bbb234153bd5', '1', 'qtalkadmin_pwd_salt_d2bf42081aab47f4ac00697d7dd32993', 'qtalk');
```

## insert a vcard
```
insert into vcard_version (username, version, profile_version, gender, host, url) values ('file-transfer', '1', '1', '1', 'qtalk', '/file/v2/download/avatar/new/daa8a007ae74eb307856a175a392b5e1.png?name=daa8a007ae74eb307856a175a392b5e1.png&file=file/daa8a007ae74eb307856a175a392b5e1.png&fileName=file/daa8a007ae74eb307856a175a392b5e1.png');
```

## db schema

host_users:
id	name	        description	                        type	
1	host_id	        domain id	                        bigint
2	user_id	        user id	                            text
3	user_name	    user name	                        text
4	department	    department	                        text
5	tel	            telephone No.                       text
6	email	        Post No.	                        text
7	dep1	        department level 1                   text
8	dep2	        department level 2                  text
9	dep3	        department level 3                  text
10	dep4	        department level 4                  text
11	dep5	        department level 5                 text
12	pinyin	        Chinese Pinyin                     text
13	frozen_flag	    frozen user flag                  smallint
14	version	        data version(for update)	      integer
15	user_type	user type（U）	                        character
16	hire_flag	 employed 	                            smallint
17	gender	     gender,1（man）2（woman）0（undefine   smallint
18	password	password	                                text
19	initialpwd	is pwd initiailized                 	smallint
20	ps_deptid	based department id	                        text
21  pwd_salt    password salt                       character varying(200)
```

[Content](README-en.md#Contents)