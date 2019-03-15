---
title: iOS单独集成QQ分享
tags:
  - iOS开发
  - 
categories:
  - 分享集成
  - 
date: 2018-09-10 10:57:26
---



官方的文档看得实在头疼，整理一份自己踩坑之后的非官方集成文档

<!-- more -->

### 下载SDK资源

链接在此   [QQSDK](http://wiki.connect.qq.com/sdk下载)

1.去[腾讯开放平台](http://op.open.qq.com/)注册账号以及创建自己的应用，创建应用之后会有审核期，一般两天左右，创建后会得到`APP ID`，在本文第4步会用到。


2.导入`TencentOpenAPI.framework`,并添加依赖库`SystemConfiguration.framework`和`libc++.tbd`

3.在`info.plist`文件中添加白名单,
此处白名单列表参考了极光分享的文档
```
<!-- QQ、Qzone URL Scheme 白名单-->
    <string>mqqapi</string>
    <string>mqq</string>
    <string>mqqOpensdkSSoLogin</string>
    <string>mqqconnect</string>
    <string>mqqopensdkdataline</string>
    <string>mqqopensdkgrouptribeshare</string>
    <string>mqqopensdkfriend</string>
    <string>mqqopensdkapi</string>
    <string>mqqopensdkapiV2</string>
    <string>mqqopensdkapiV3</string>
    <string>mqqopensdkapiV4</string>
    <string>mqzoneopensdk</string>
    <string>wtloginmqq</string>
    <string>wtloginmqq2</string>
    <string>mqqwpa</string>
    <string>mqzone</string>
    <string>mqzonev2</string>
    <string>mqzoneshare</string>
    <string>wtloginqzone</string>
    <string>mqzonewx</string>
    <string>mqzoneopensdkapiV2</string>
    <string>mqzoneopensdkapi19</string>
    <string>mqzoneopensdkapi</string>
    <string>mqqbrowser</string>
    <string>mttbrowser</string>
```

4.在`info.plist`文件下的`URL Types`里加入设置`URL Schemes`

`URL Schemes`设置为`tencent+APPID`


5.在需要分享的VC中导入头文件

```
#import <TencentOpenAPI/QQApiInterface.h>
#import <TencentOpenAPI/QQApiInterfaceObject.h>
```


6.分享代码

>分享文本到QQ联系人


```
QQApiTextObject *txtObj = [QQApiTextObject objectWithText:@"分享测试"];
SendMessageToQQReq *req = [SendMessageToQQReq reqWithContent:txtObj];
// 将内容分享到qq
QQApiSendResultCode sent = [QQApiInterface sendReq:req];
```

待续

