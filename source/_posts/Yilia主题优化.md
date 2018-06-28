---
title: Yilia主题优化
date: 2018-06-28 11:03:00  // 指定发布日期
tag: yilia优化
category: Hexo
toc: true  // 开启文章标签
---

Hexo主题yilia持续优化中...

<!-- more -->

## 给博客增加动态标签

效果如下

![link](https://github.com/Ziven-n/Blog-Back-Up/blob/master/source/picture/Yilia%E4%B8%BB%E9%A2%98%E4%BC%98%E5%8C%96/%E5%8A%A8%E6%80%81%E6%A0%87%E7%AD%BE%E6%95%88%E6%9E%9C.gif?raw=true)

##### 实现方式

打开博客路径themes/yilia/layout/layout.ejs

在“</body>”前添加如下代码即可


```
<script type="text/javascript">var OriginTitile=document.title,st;document.addEventListener("visibilitychange",function(){document.hidden?(document.title="看不见我🙈~看不见我🙈~",clearTimeout(st)):(document.title="(๑•̀ㅂ•́) ✧被发现了～",st=setTimeout(function(){document.title=OriginTitile},3e3))})</script>
```


## 给博客增加点击小红心效果

效果如下

![link](https://github.com/Ziven-n/Blog-Back-Up/blob/master/source/picture/Yilia%E4%B8%BB%E9%A2%98%E4%BC%98%E5%8C%96/%E7%82%B9%E5%87%BB%E5%B0%8F%E7%BA%A2%E5%BF%83%E6%95%88%E6%9E%9C.gif?raw=true)





##### 实现方式

在 themes/yilia/source 文件目录下增加[love.js](https://github.com/Ziven-n/Blog-Back-Up/blob/master/themes/yilia/source/love.js)文件

然后在themes/yilia/layout/_partial/footer.ejs 文件， 在文件的最后， 添加以下代码：


```
<!--页面点击小红心-->
<script type="text/javascript" src="/love.js"></script>

```

