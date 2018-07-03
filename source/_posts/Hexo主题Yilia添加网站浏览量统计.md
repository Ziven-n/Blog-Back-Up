---
title: Hexo主题Yilia添加网站浏览量统计
date: 2018-06-25 15:13:00  
tag: 网站统计
category: Hexo
---


## 方式
>常见的几种方式如下


<!-- more -->

1. 不蒜子
2. 友盟
3. LeanCloud




这里推荐不蒜子的脚本统计

## 操作步骤





首先去[不蒜子官网](http://busuanzi.ibruce.info/)复制以下代码


```
<script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

打开自己的blog本地仓库路径

```
hexo\themes\yilia\layout_partial\footer.ejs
```
在footer.ejs文件最后粘贴此行代码，将脚本添加至自己的博客项目中


然后根据个人喜好添加以下代码

>本站总访问量
```
<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>
```

>本站访客数
```
<span id="busuanzi_container_site_uv">
  本站访客数<span id="busuanzi_value_site_uv"></span>人次
</span>
```


>本文总阅读量
```
<span id="busuanzi_container_page_pv">
  本文总阅读量<span id="busuanzi_value_page_pv"></span>次
</span>
```
以下是本人自己的配置


![link](https://github.com/Ziven-n/Blog-Back-Up/blob/master/source/picture/Hexo添加统计-1.png?raw=true)



然后重新用命令一键部署博客即可

```
hexo clean && hexo g -d
```


等几分钟就可以看到统计效果了
