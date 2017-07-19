---
layout: post
title: "api blueprint 使用手册"
excerpt: "文档描述的是如何使用api blueprint编写api文档。" 
categories: articles
tags: ['api-blueprint', 'markdown', 'web', 'RESTful', 'agilio']
comments: true
share: true
image:
  feature: so-simple-sample-image-7.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

最近一段时间在整理fuel相关的api， 但是官方没有太完整的api文档；所以需要一个api接口管理程序来整理fuel api。 

看到github上关于api-blueprint的相关介绍， 由于它使用markdown来编写api文档， 会比较容易上手。 

总结一下使用方法和填的一些坑。 


# 工作环境

`Ubuntu 16.04 + Vim`

可以根据选择自己喜欢的操作系统和编辑器。 


# 生成HTML文件

agilo 是一个将根据api-blueprint规范的markdown文件转换为HTML工具。 

安装和使用非常简单(依赖nodejs)

```
npm install -g aglio

agilo -i hello.md -o hello.html

```

> Note: 由于ubuntu平台为了避免和其他被称为:node的软件冲突， 所以命名：nodejs, 这样在按照agilo时候是报错： node not found. 
> 我们可以通过创建一个软链接来workaround. 
> `sudo ln -s "$(which nodejs)" /usr/bin/node`

 * [官网](https://apiblueprint.org/)
 * [Tutorial](https://apiblueprint.org/documentation/tutorial.html)
 * [渲染工具-aglio](https://github.com/danielgtaylor/aglio)
 * [语法文档](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md)
 * [示例](https://github.com/apiaryio/api-blueprint/tree/master/examples)
 * [MSON](https://github.com/xiaoyao9184/mson)
 * [Mock Server-drakov](https://github.com/Aconex/drakov)
