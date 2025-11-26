---
title: flutter插件image_picker选择视频耗时长的问题
tags:
  - Flutter
  - image_picker
categories:
  - App开发
  - 插件
date: 2025-11-26 14:15:22
---


最近在使用```image_picker```插件获取本机相册时发现，选择完相册视频后会需要处理很久才会返回视频的路径，而且仅仅只针对.mov格式的视频才会这样，平时1秒内可以选择完成的视频现在需要30S以上
<!-- more -->

这个问题官方也有相关讨论
[https://github.com/flutter/flutter/issues/176355](https://github.com/flutter/flutter/issues/176355)




#### 解决方案如下

从```image_picker```迁移到```file_picker```

1.导入库```file_picker: ^10.3.6```
2.获取对应文件，FileType.media表示选择系统相册，里面会包含图片跟视频，也可以单独设置video或者image
```
final result = await FilePicker.platform.pickFiles(
      type: FileType.media, // 系统默认的媒体选择器
      allowMultiple: false,
      withData: false,
    );
    
File file = File(result.files.single.path!);
```
3.最终获取到的文件是未经压缩的图片或者视频，也正是没有压缩转码这种过程，所以可以快速选择完返回路径，这样获取的文件在上传时可能会遇到服务器不支持的格式，比如.mov的视频，无法通过```video_player```在安卓机上播放，.heic文件格式的图片服务器不支持。选择完成后需要单独处理一下，这里借助一下两个库
```video_compress: ^3.1.4```
```flutter_image_compress: ^2.4.0```

4.定义两个新方法分别处理图片和视频

```
Future<File?> compressImage(File file) async {
    // print('开始处理图片');
    // final dir = file.parent.path;
    // final nameNoExt = p.basenameWithoutExtension(file.path);
    // final targetPath = p.join(dir, '$nameNoExt.jpg');

    final tempDir = await getTemporaryDirectory();
    final targetPath = "${tempDir.path}.jpg";

    var result = await FlutterImageCompress.compressAndGetFile(
      file.absolute.path,
      targetPath,
    );

    if (result == null) {
      return null;
    }

    // print('图片处理完成${result.path}');
    return File(result.path);
  }
```


```
Future<File?> compressVideo(File file) async {
    // print('开始处理视频');
    MediaInfo? mediaInfo = await VideoCompress.compressVideo(
      file.path,
      quality: VideoQuality.DefaultQuality,
      deleteOrigin: false,
    );
    if (mediaInfo == null) {
      return null;
    }
    // print('视频处理完成${mediaInfo.file?.path}');
    return File(mediaInfo.path!);
  }
```
5.最终就可以得到通用的格式.jpg和.mp4了,速度也非常快