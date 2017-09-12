---
title: 搭建Material主题的Hexo博客
date: 2017-07-18 13:58:43
tags:
---


全篇可以在这搜到：

[戳这里](https://material.viosey.com/services/)

## 本地搜索插件
npm install hexo-generator-search --save
在主配置文件中添加：
search:
    path: search.xml
    field: all
    
    
## rss订阅插件
npm install hexo-generator-feed --save

feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content: