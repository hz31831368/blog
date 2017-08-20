---
title: Hexo Markdown代码块显示不正常
date: 2017/8/20 23:45:00
tags: issue
---

今天转载Java内存模型的时候发现代码块错乱，如图所示  
![image](http://on3qybwfn.bkt.clouddn.com/QQ截图20170820233630.png)  
  
原因是写完代码块之后习惯性按markdown语法按了两个空格加回车，代码块语法后面多了那两个空格之后导致解析异常。   

![image](http://on3qybwfn.bkt.clouddn.com/QQ截图20170820233650.png)  
  

这个应该跟Markdown解析器有关系，我用有道云笔记打的草稿是没有问题的，部署到hexo就有问题了。
