---
title: APP在真机运行时卡在启动的问题
tags:
  - Flutter
  - Xcode
categories:
  - App开发
date: 2025-08-22 17:57:22
---


最近由于iOS18.5发热严重，将iPhone的系统升级到了iOS18.6.2，升级完后app在真机调试时发现，会一直卡在启动页前面的白屏，大概会卡2分钟以上，才会开始进入启动页。仔细观察后发现控制台有出现一个警告：
<!-- more -->

```
libobjc.A.dylib is being read from process memory. This indicates that LLDB could not find the on-disk shared cache for this device. This will likely reduce debugging performance
```

#### 解决方案如下

1.先关闭Xcode

2.从Mac桌面进入以下路径的文件夹

```~/Library/Developer/Xcode/iOS DeviceSupport/```

3.清空文件夹中的所有内容

4.重新打开Xcode

5.等待Xcode下载完成后问题解决


参考：
[stackoverflow](https://stackoverflow.com/questions/72598348/xcode-14-beta-build-issues-with-lldb)

