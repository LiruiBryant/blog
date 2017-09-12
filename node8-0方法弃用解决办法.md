---
title: node8.0方法弃用解决办法
date: 2017-07-18 11:15:25
tags: nodejs
---


### 刚升级node后发现报如下的错：
* (node:34880) [DEP0061] DeprecationWarning: fs.SyncWriteStream is deprecated.

    node.js从8.0开始已经弃用了fs.SyncWriteStream方法，hexo中有一个hexo-fs插件，调用了这个方法，所以就会报错。
    
### 解决办法：

* npm install hexo-fs --save

插件就更新了，问题就解决了。