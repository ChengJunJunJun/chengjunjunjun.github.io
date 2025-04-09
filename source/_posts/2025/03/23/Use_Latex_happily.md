---
title: Latex 基础知识和常用命令解释
date: 2025-03-23
last_modified: 2025-03-23
author: Cheng Jun
desc: Latex 基础知识和常用命令解释
tags: [Latex, command, tutorial]
categories: Technical sharing
---

#### 什么是Latex？

Latex 是一种基于TeX的排版系统，广泛用于生成高质量的科技论文、书籍和报告。它提供了丰富的命令和宏包，可以方便地创建复杂的数学公式、图表和参考文献。（AI自动生成）

掌握Latex非常重要，但是目前大部分研究生都是用overleaf【一个在线的Latex编辑器】写论文。__知其然不知其所以然__。

下面介绍常见的使用Latex的流程，从搭建环境到写论文。（持续更新，因为我自己也是在学习阶段）

#### Mac搭建Latex环境

1，使用homebrew安装mactex-no-gui【一个Latex的发行版,没有图形界面】，按照提示安装，配置环境变量。常见的命令就可以找到了。

```bash
brew install --cask mactex-no-gui
```

2，建议使用VScode或者是cursor写Latex，安装插件：LaTeX Workshop。

#### 如何使用LaTeX Workshop？（详细介绍一下配置文件的设置）
我现阶段的配置文件如下：
```json

// === LaTeX Workshop Plugin Settings ===
    "latex-workshop.latex.recipes": [
        {
            "name": "XeLaTeX+Biber+XeLaTeX_2", // 这个配置应该是写论文最常见的配置，下面会有介绍
            "tools": [
                "xelatexmk",
                "biber",
                "xelatexmk",
                "xelatexmk"
            ]
        },
        {
            "name": "XeLaTeX",
            "tools": [
                "xelatexmk"
            ]
        },
        {
            "name": "PdfLaTeX",
            "tools": [
                "pdflatexmk"
            ]
        }
    ]
```

- 注：XeLaTeX+Biber+XeLaTeX_2
    - XeLaTeX：一个基于XeTeX的LaTeX引擎，支持Unicode字符集，可以处理中文、日文、韩文等语言。
    - Biber：一个用于处理BibTeX文献数据库的工具，可以生成参考文献列表。
    - 编译流程：
        - 运行XeLaTeX（生成辅助文件）
        - 运行Biber（处理参考文献）
        - 再次运行XeLaTeX（整合参考文献）
        - 最后再运行一次XeLaTeX（确保交叉引用正确）