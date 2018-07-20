---
title: SpriteKit 中的anchorPoint和position
tags:
  - SpriteKit
  - iOS游戏开发
categories:
  - anchorPoint
  - position
date: 2018-07-19 17:13:13
---


最近打算开始用`SpriteKit`做一个像素鸟的小游戏，在此记录一下学习及踩坑的过程。第一个坑：`anchorPoint`和`position`。

<!-- more -->



## `anchorPoint`

锚点，在`iOS`系统中，`anchorPoint`默认位于图层的中心点，`anchorPoint`的范围是`(0，0)～(1，1)`，中心点就是`(0.5，0.5)`,在`iOS`里，坐标系是左手坐标系，坐标原点在左上角，而`SpriteKit`中的坐标系是右手坐标系同`Mac OS`，原点在左下角.找了个图如下，很清晰

![link](https://github.com/Ziven-n/Blog-Back-Up/blob/master/source/picture/anchorPoint/%E5%9D%90%E6%A0%87%E7%B3%BB.png?raw=true)

## `position`
理解为锚点这个点在坐标系中所处的位置。无需赘述。
