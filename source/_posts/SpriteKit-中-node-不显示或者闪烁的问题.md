---
title: SpriteKit 中 node 不显示或者闪烁的问题
tags:
  - iOS游戏开发
  - SpriteKit
categories:
  - zPosition
  
date: 2018-07-20 15:24:52
---


今天遇到了第二个坑，`zPosition`的问题

<!-- more -->

在游戏开发过程中，创建的小鸟`node`节点在执行动画的时候闪烁，甚至有时候不显示，创建上下水管的过程中也是，水管不显示，或者随机显示，`FPS`值稳定`60`，但是就是有问题，`google`了一下发现，跟`iOS`里不同，在`iOS`中，控件添加到父视图上时，会有先来后到的顺序，后添加的会处于最上层，而`SpriteKit`里则不一样，即使你后添加的顺序也不是一定的，需要设置`zPosition`将节点`node`的层级手动提升.

## zPosition
在`SpriteKit`中，每个节点都有一个的属性`zPosition`，默认值为`0`，之前创建`SKView`时，我们将其属性`ignoresSiblingOrder`设置为`Yes`，这才导致了顺序的随机性，设置`ignoresSiblingOrder`为`No`,可以使节点`node`按照先来后到的顺序绘制。

> 默认情况下`ignoresSiblingOrder`设置为`Yes`，因为它允许`SpriteKit`完成潜在的性能优化使游戏运行的更快.

> 鉴于此，可以将游戏默认背景节点的`zPosition`设置为小于`0`的值
