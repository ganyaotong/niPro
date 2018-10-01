---
title: "手把手教你创建一个hugo主题"
date: 2018-10-01T11:25:07+08:00
description: ""
Tags: [""]
Categories: [""]
draft: true
---

这篇文章翻译自：https://yashagarwal.in/posts/2018/03/develop-a-theme-for-hugo/

# 简介
在这个教程，我通过案例教你如何创建一个简单hugo主题。假设你已经熟悉基本的HTML，与使用Markdown语言创建内容。我将会解释hugo的工作原理，以及怎样使用go模板语言，还有如何使用这些模板组织你的内容。在篇文章集中hugo的工作，不会涉及CSS。
我们将会开始学习hugo创建的一些必要信息。然后使用非常基础模板创建一个hugo网站。然后我们添加新的主题与发布一篇文章到我们的网站。再进一步探究hugo。一些非常微小的变化，在这里你将会学习什么，你将会创建一个不同类型的真实网站。
好了，关于这篇教程的流程。命令开始符合$,他的意思是你正在运行终端或者命令行。命令是马上输出的。注释使用#号开始。

# 一些术语
## 配置文件
hugo网站使用配置文件识别公共设置。它的位置位于你的网站跟目录，它支持TOML，YAML，与JSON格式，hugo使用扩展识别这些文件。
默认，hugo需要在content/目录查找Markdown文件与在theme/目录查找模板文件。它将会在public/目录创建HTML文件。你可以在配置文件特别指定来更改这些位置。

## 内容
内容文件里面包含元数据与你的内容，内容文件全部可以分为连个部分，顶部是摘要，下一个部分是markdown，hugo会把这两部分转换成html。内容文件位于/content目录。

## 摘要
摘要部分包括你文章的信息，可以使用JSON，TOML或者YAML来书写。hugo使用tokens（标记）帮助识别摘要的不同类型。TOML是+++包围，YAML是---包围，JSON是{}包围。

我喜欢使用YAML，如果你使用的是JSON或者TOML，你需将配置转换过来。

这是使用YAML书写的摘要。
--
---
date: "2018-02-11T11:45:05+05:30"
title: "简单hugo主题教程"
description: "一个hugo主题入门书，静态网站生成器，使用golang开发。"
categories:
    - Hugo
    - 自定义
tags:
    - 主题
---
--
你可以阅读更多不同的配置选项，这里[https://gohugo.io/content-management/front-matter/#readout]

## Markdwon
Markdown部分是你书写的部分，hugo会使用Markdown引擎自动将其转化为html。

## 模板
