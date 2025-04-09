---
title: 博客搭建指南
date: 2025-01-04
last_modified: 2025-01-04
author: Cheng Jun
desc: 2025年终于找到了自己喜欢的博客主题，开始写一些技术博客了！
tags: [Hexo, GitHub, Blog]
categories: Technical sharing
---

#### 一点点前提

在搭建博客之前，首先需要了解什么是Node.js 项目。这个完全可以ChatGPT，Node.js 项目其实就是JavaScript 运行环境。下面是一个Node.js 项目的文件结构：
```
typical-node-project/
├── node_modules/          # 依赖包目录
├── package.json          # 项目配置文件
├── package-lock.json     # 依赖版本锁定文件
├── src/                  # 源代码目录
└── .gitignore           # Git忽略文件
```
可以使用`npm（Node Package Manager）`管理依赖，`node_modules `文件夹是 `Node.js` 项目中存放所有依赖包（第三方库）的地方。


#### 博客框架

个人觉得写博客和开发网站不同，写博客主要是内容的输出，没有必要从头到尾自己搭建，所以使用一个博客框架就很重要，我使用的是[Hexo](https://github.com/hexojs/hexo)
	Hexo 是一个快速、简洁且高效的博客框架。 Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他标记语言）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
直接看它的官方文档就可以愉快的使用了。

#### 一个好看的主题

在大概了解相关的知识之后，就是选择一个好看的主题，这个是博客的门面，我的主题是[Typography](https://github.com/SumiMakito/hexo-theme-typography)但是我最开始不是看github发现的，主要是看B站的[polebug23](https://space.bilibili.com/58078997/dynamic?spm_id_from=333.1365.list.card_avatar.click)。非常热爱学习的一位up主。