---
title: 不蒜子与live2d冲突的问题
tags:
  - 网站统计
  - live2d
categories:
  - Hexo
date: 2025-05-23 18:19:22
---


最近更新博客时发现使用live2d后会导致不蒜子的统计功能失效
<!-- more -->

##### 解决方案如下

> 1.通过增加一次不蒜子的配置去冲突，让真正需要显示的不蒜子统计显示出来
 
 在```yilia/layout/_partial/article.ejs```文件里新增以下代码，注意是额外增加的，需要在你项目中已配置不蒜子的地方额外再配置一次

```
<div style="display: none;">
  <span id="busuanzi_container_site_uv"></span><span id="busuanzi_container_site_uv"></span>
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

>2.将文字前的span去掉，可以防止被冲突
```
<!-- <span id="busuanzi_container_site_pv"> -->
总访问量: <span id="vercount_value_site_pv"></span> |
</span>
```
>3.更换统计方式
 
 将统计方式由不蒜子迁移到[Vercount](https://vercount.one/#usage)
 
具体使用方法与不蒜子一致：

先在```footer.ejs```文件添加：

```
<script defer src="https://events.vercount.one/js"></script>
```

再将不蒜子相关的span id改为vercount相关的
```
Total Page View <span id="vercount_value_page_pv">Loading</span>

Total Visits <span id="vercount_value_site_pv">Loading</span>

Site Total Visitors <span id="vercount_value_site_uv">Loading</span>
```

第3种方式会将之前的统计量清零，介意的可以不修改span id
vercount会兼容不蒜子的统计，只修改```footer.ejs```文件即可