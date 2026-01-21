---
title: flutter开发环境配置
tags:
  - 环境配置
categories:
  - Flutter
date: 2026-01-21 16:50:22
---


最近要换电脑重新运行已有项目，正好整理一下开发环境的搭建流程。
<!-- more -->

[flutter中文网](https://docs.flutter.cn/get-started)




****1.先下载flutterSDK****

下载链接：[flutterSDK](https://docs.flutter.dev/install/archive?tab=macos)

可在链接中下载指定版本，下载完成后在桌面新建文件夹```flutterSDK``` ，将整个flutter文件夹放进去。也可以放其他位置，但是要注意后面环境配置及签名文件的路径。

#### 注：M芯片的Mac下载arm64版本，否则是x64

****2.下载```vscode```和```android studio```****

下载链接：
[vscode](https://code.visualstudio.com/)
[android studio最新版本](https://developer.android.com/studio?hl=zh-cn)
[android studio历史版本](https://developer.android.google.cn/studio/archive)

#### 注：M芯片的Mac下载apple芯片版本，否则是Intel芯片版本，历史版本网页如果不是英文可能显示有问题

****3.vscode插件安装****

必需的：
```Flutter```
```Dart```

推荐的：
```Atom One Dark Theme``` 
```Flr```
```Chinese```

****4.电脑环境配置****

终端执行：```open ~/.bash_profile```
如果没有需要创建```touch .bash_profile```

将以下内容复制进去
```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export PATH=/Users/ziven/Desktop/flutterSDK/flutter/bin:$PATH
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
``` 
#### 注：```Users/ziven/Desktop/flutterSDK```这个路径不可以写错

保存：```source ~/.bash_profile```

****5.设置flutter命令全局可用****

执行：
```open ~/.zshrc``` 

如果 提示文件不存在，则新建一个新文件。
```vim ~/.zshrc ```

再执行
```open ~/.bash_profile```

把 bash_profile 中的内容copy到 zshrc 文件中，保存：【:wq回车】

再使用source命令重新加载一下
```source ~/.zshrc```

运行命令看生效没有
```flutter -h```

.zshrc文件的内容大概是这样：底下是go语言相关的环境配置可忽略
```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export PATH=/Users/ziven/Desktop/flutterSDK/flutter/bin:$PATH
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"  

export PATH="$PATH":"$HOME/.pub-cache/bin"

export GOPATH=$HOME/go
export GOROOT="$(brew --prefix golang)/libexec"
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# 设置 Go 代理（国内建议使用）
export GO111MODULE=on
export GOPROXY=https://goproxy.cn,directexport PATH=$PATH:/Users/ziven/Library/Android/sdk/platform-tools
```

****6.终端运行```flutter doctor```****
将已知问题全部解决


****7.配置安卓签名****
Mac终端执行命令：
```keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key```
生成的```key.jks```文件也同样放在桌面

在```build.gradle.kts```文件中新增配置

```
val keystoreProperties = Properties()
val keystorePropertiesFile = rootProject.file("key.properties")
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(FileInputStream(keystorePropertiesFile))
}
```

```
signingConfigs {
        create("release") {
            keyAlias = keystoreProperties["keyAlias"] as String
            keyPassword = keystoreProperties["keyPassword"] as String
            storeFile = keystoreProperties["storeFile"]?.let { file(it) }
            storePassword = keystoreProperties["storePassword"] as String
        }
    }

    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
            isMinifyEnabled = true      // 开启代码混淆
            isShrinkResources = true    // 开启资源压缩
            // proguardFiles(
            // getDefaultProguardFile("proguard-android.txt"),
            // "proguard-rules.pro"
        // )

        }
        debug{
            // debuge模式也使用证书
            signingConfig = signingConfigs.getByName("release")
        }
    }
```

新增```key.properties```文件，放入证书相关内容
```
storePassword=******
keyPassword=******
keyAlias=key
storeFile=/Users/ziven/Desktop/***/key.jks
```

****8.打包运行****
