---
title: 不蒜子与live2d冲突的问题
tags:
  - 网站统计
  - live2d
categories:
  - Hexo
date: 2025-05-23 18:19:22
---




####  最近更新博客时发现使用live2d后会导致不蒜子的统计功能失效

##### 解决方案如下
<!-- more -->
 
 在```yilia/layout/_partial/article.ejs```文件里新增以下代码，注意是额外增加的，需要在你项目中已配置不蒜子的地方额外再配置一次

```
<div style="display: none;">
  <span id="busuanzi_container_page_pv"></span><span id="busuanzi_container_site_pv"></span>
</div>
```

 在```yilia/layout/_partial/footer.ejs```文件里新增以下代码

```
<div style="display: none;">
    <span id="busuanzi_container_site_uv"></span><span id="busuanzi_container_site_pv"></span>
</div>
```
其他已配置的不蒜子统计不要动

原因参考：
[busuanzi 访问量统计与 live2d 插件同时使用导致 busuanzi 不显示的根本原因以及解决方法](https://ouuan.moe/post/2022/08/busuanzi-and-live2d)

本文中的解决方案可能更简单一点