---
title: Hexo主题yilia增加gitalk评论插件
date: 2018-07-03 15:37:30
tags: gittalk
categories: Hexo
---

之前使用的`gitment`没有更新了，而且移动端适配做的不够好，故将评论功能迁移至`gitalk`,以下是替换过程

<!-- more -->

## 在`layout/_partial/post`目录下新增`gitalk.ejs`文件

>文件内写入如下代码：

```
<div id="gitalk-container" style="padding: 0px 30px 0px 30px;"></div> 

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
<script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
<script type="text/javascript">

if(<%=theme.gitalk.enable%>){
	var gitalk = new Gitalk({
  	clientID: '<%=theme.gitalk.ClientID%>',
  	clientSecret: '<%=theme.gitalk.ClientSecret%>',
  	repo: '<%=theme.gitalk.repo%>',
  	owner: '<%=theme.gitalk.githubID%>',
  	admin: ['<%=theme.gitalk.adminUser%>'],
  	id: '<%= page.date %>',
  	distractionFreeMode: '<%=theme.gitalk.distractionFreeMode%>'
})
gitalk.render('gitalk-container') 
}
</script>
```

## 修改`source-src/css`/目录下`comment.scss`文件


```
#disqus_thread, .duoshuo, .cloud-tie-wrapper, #SOHUCS, #gitment-ctn, #gitalk-container {
	padding: 0 30px !important;
	min-height: 20px;
}

#SOHUCS {
	#SOHU_MAIN .module-cmt-list .block-cont-gw {
		border-bottom: 1px dashed #c8c8c8 !important;
	}
}
```

## 在`layout/_partial`目录下的`article.ejs`文件内新增`gitalk`相关的配置代码：

```
<% if(theme.gitalk.enable){ %>
  <%- partial('post/gitalk', {
      key: post.slug,
      title: post.title,
      url: config.url+url_for(post.path)
    }) %>
  <% } %>
```

## 最后在`yilia`主题配置文件中新增`gitalk`相关的配置：


```
#6、gitalk评论
gitalk:
  enable:  true
  githubID:  Ziven-n   
  repo:  'mycomments'
  ClientID:  '29fd06ce234338fe233'
  ClientSecret:  '090b271a3e6ce6fae6fc80ce4d4344671aa'
  adminUser:  Ziven-n
  distractionFreeMode: true
```

## 更新一下即可
