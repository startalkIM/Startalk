[返回首页](https://github.com/startalkIM/Startalk)

# 简介
如果您按照后端的部署文档，或者使用我们的一键部署包完成了部署，那么只看这篇文档就可以 完善push能力了，并不需要单独下载和部署任何内容。

IM后端的push服务，支持IOS apns协议推送，android 小米，华为，魅族，oppo厂商推送 push服务支持qtalk消息push，同时也支持接入自己的push服务

#私有化部署push服务配置

全程只需要修改一个配置文件，重启下服务即可。

文件位置在：

/startalk/tomcat/push_service/webapps/push_service/WEB-INF/classes/app.properties

### Android和IOS证书配置

如果需要使用服务支持的push，Android需要自己去小米和华为开发平台注册自己应用的app_key,Ios需要生产签名证书，配置如下：

#### ios push 证书

```
ios_push_cer_qtalk=线上证书所在路径
ios_push_cer_qtalk_beta=beta证书所在路径
```

#### Android配置

```
adr.qtalk.pkgname=应用包名
adr.mipush.qtalk.key=mipush key(是个文本)
adr.hwpush.qtalk.key=hwpush key(是个文本)
```

***

# 注意事项

#### Android需要在项目的gradle文件中添加对应厂商push信息
```
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
                    currentPlat  : "StarTalk",
                    MAIN_SCHEMA : "start_qtalk_activity"
            ]
        }
```

[返回首页](https://github.com/startalkIM/Startalk)