---
title: flutter接入FCM推送通知
tags:
  - FCM
  - 推送
categories:
  - flutter
  - firebase
date: 2025-07-03 11:19:22
---

#### 集成依赖
```
firebase_messaging: ^15.2.7
flutter_local_notifications: ^19.2.1
```
<!-- more -->

如果前面有使用firebase相关的功能比如```firebase_auth```、```firebase_core```，会需要升级，一般建议全都使用最新的,
firebase相关的集成这里先默认都已经集成完毕

firebase如果已经集成完毕，会在项目中各处地方看到```google-services.json```文件，里面有你在firebase控制台创建项目的相关信息

####  写一个工具类来处理推送相关的设置

```
import 'dart:convert';
import 'dart:io';
import 'package:firebase_messaging/firebase_messaging.dart';
import 'package:flutter/foundation.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';
import 'package:spotito/common/head.dart';

class PushManager {
  factory PushManager() => _instance;

  PushManager._();

  static final PushManager _instance = PushManager._();

  //FCM SDK对象
  final _firebaseMessaging = FirebaseMessaging.instance;

  //Notifications插件对象
  final _localNotifications = FlutterLocalNotificationsPlugin();

  //Android 8+ 的消息通道
  final _androidChannel = const AndroidNotificationChannel(
    'default_channel', //设置 Channel ID
    'Default Notifications', //设置 Channel 名称
    importance: Importance.max, // 如果是default则不会有横幅提醒
  ); //设置该 Channel 的优先级

  // 初始化，获取设备token
  Future<void> initPush() async {
    //申请动态通知权限
    await _firebaseMessaging.requestPermission();
    _firebaseMessaging.setAutoInitEnabled(false);
    //获取 FCM Token
    final fCMToken = await _firebaseMessaging.getToken();
    if (fCMToken != null) {
      debugPrint("获取到FCMToken:$fCMToken");
    }
    //刷新 FCM Token 的监听
    _firebaseMessaging.onTokenRefresh.listen((newToken) {
      debugPrint("获取到刷新后的FCMToken:$newToken");
    });

    //拿到 FCM Token 之后初始化一些监听
    _initPushNotifications();
    _initLocalNotifications();
  }

  /// Notifications 插件初始化，监听前台消息
  Future _initLocalNotifications() async {
    const iOS = DarwinInitializationSettings();
    // 图标需要是白色透明背景否则会显示不出来
    const android = AndroidInitializationSettings('push_icon2');
    // Notifications 插件只需要处理 Android 和 iOS 平台的配置
    const settings = InitializationSettings(android: android, iOS: iOS);
    // Notifications 插件初始化
    await _localNotifications.initialize(
      settings,
      onDidReceiveNotificationResponse: (NotificationResponse response) {
        // android 前台消息点击回调
        final message = RemoteMessage.fromMap(jsonDecode(response.payload!));
        // 处理收到消息
        handleMessage(message);
      },
    );
    //Android 8 以上会通过 Channel 创建对应的通知渠道
    final platform =
        _localNotifications
            .resolvePlatformSpecificImplementation<
              AndroidFlutterLocalNotificationsPlugin
            >();
    await platform?.createNotificationChannel(_androidChannel);
  }

  /// FCM 初始化接收消息的各种回调
  Future _initPushNotifications() async {
    await _firebaseMessaging.setForegroundNotificationPresentationOptions(
      alert: true,
      badge: true,
      sound: true,
    );

    // 打开app时，会执行该回调，获取消息（通常是程序终止时，点击消息打开app的回调）
    _firebaseMessaging.getInitialMessage().then((RemoteMessage? message) {
      if (message == null) return; // 没有消息不执行后操作
      handleMessage(message);
    });

    // 后台程序运行时，点击消息触发
    FirebaseMessaging.onMessageOpenedApp.listen(
      (RemoteMessage message) => handleMessage(message),
    );

    // 前台消息，android不会弹出通知框，所以需要 Notifications 插件自定义本地通知（iOS没有前台消息，iOS的前台消息和后台运行时一样的效果）
    FirebaseMessaging.onMessage.listen((message) {
      debugPrint("前台收到推送消息=======${message.notification}");
      final notification = message.notification;
      if (notification == null) return;
      if (Platform.isIOS) return;
      _localNotifications.show(
        notification.hashCode,
        notification.title,
        notification.body,
        NotificationDetails(
          android: AndroidNotificationDetails(
            _androidChannel.id,
            _androidChannel.name,
            icon: 'push_icon2',
          ),
        ),
        payload: jsonEncode(message.toMap()),
      );
    });
  }

  // 处理收到的消息，比如跳转页面或者其他处理
  void handleMessage(RemoteMessage message) {
    final dataJson = message.data;
    debugPrint("点击收到的推送消息:$dataJson");
  }
}

```

####  在```Main.dart```中新增初始化代码
```
PushManager().initPush();
```
如果前面有集成firebase相关的功能，写在

```
await Firebase.initializeApp(
        options: DefaultFirebaseOptions.currentPlatform,
      );
```
后面即可

----

#### 注意事项

1.推送图标在安卓上需要单独设置，iOS不需要，单独让设计出一张白色透明背景的图，建议48*48的尺寸，太小了会显示不清，图片放在```android/app/src/main/res/drawable```文件夹下面，设置的时候直接写图片名称就可以了

2.获取到的Token会刷新，针对安卓，可以在```AndroidManifest.xml```中新增

```
        <meta-data
            android:name="firebase_messaging_auto_init_enabled"
            android:value="false" />
        <meta-data
            android:name="firebase_analytics_collection_enabled"
            android:value="false" />

```
iOS上需要在```info.plist```文件中新增
```
FirebaseMessagingAutoInitEnabled = No
```

3.安卓集成```flutter_local_notifications```时，需要额外配置脱糖

```
compileOptions {
    isCoreLibraryDesugaringEnabled = true
    ...
}
```

```
dependencies {
  // 脱糖库 需要搭配 isCoreLibraryDesugaringEnabled = true 使用
 coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.4") 
 ...
}
```

4.iOS集成时需要在AppDelegate文件中新增初始化代码
```
import Flutter
import UIKit
// 新增
import FirebaseCore

@main
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    // 新增  
    FirebaseApp.configure()
    
    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```
同时项目的```Signing & Capabilities```开启推送相关设置
在firebase上上传推送证书，或者直接使用```APNs 身份验证密钥```来配置

