---
title: 更加强大的python包管理器uv
date: 2025-05-04
last_modified: 2025-05-04
author: Cheng Jun
desc: uv 是一个更加强大的python包管理器，它可以帮助你更加方便地管理python包。
tags: [python, uv]
categories: [python]
---

#### 前言
最近的一段时间，三月份我在考雅思，中间有段时间也没有好好学技术，今天是五四青年节，我认为还是要好好沉淀下来，静下心来好好的提升自己。

**祝大家五四青年节快乐！**

#### 传统的虚拟环境管理命令
1. `python -m venv .venv` 创建虚拟环境
2. `source .venv/bin/activate` 激活虚拟环境
3. `edit pyproject.toml` 编辑配置文件
4. `pip install -e .` 下载配置文件

#### uv和conda,venv,pip的关系。
这一部分，我打算用Gemini的`deep research with 2.5 pro`来回答。

- uv是用rust写的，就是很快，目前也可以看见很多大公司或者程序员开始用uv来进行python的包管理。
- 我现在打算在一个我进行python学习的地方开始用uv来管理我的环境，conda主要用来做深度学习和机器学习。

#### uv的常用命令
我是一个homebrew用户，所以uv的安装也非常简单，只需要一行命令：

```bash
brew install uv
``` 

#### uv的常见命令
这里放一个uv官方的链接，[Working on projects](https://docs.astral.sh/uv/guides/projects/#working-on-projects)，这个部分将python的项目管理写的很清楚了。（我非常建议在初始化项目的时候，学好**git**项目的版本管理，这个对于更好的写代码至关重要。）

1. uv add
2. uv sync
3. uv run main.py

#### uv的缺点
我认为uv的主要缺点是在机器学习和深度学习领域，目前用**conda**还是很多。
















