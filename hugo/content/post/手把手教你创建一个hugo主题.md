---
title: "手把手教你创建一个hugo主题"
date: 2018-10-01T11:25:07+08:00
description: ""
Tags: ["hugo"]
Categories: ["网站开发"]
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
摘要部分包括你文章的信息，可以使用JSON，TOML或者YAML来书写。hugo使用tokens（标记）帮助识别摘要的不同类型。TOML是+++包围，YAML是---包围，JSON是{}包围。在转换为HTML的时候，将会分析内容类型摘要中的信息，在模板中使用该特定内容类型。

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
在hugo中，模板呈现方式，你的内容将会呈现为HTML，在呈现markdown内容时，每个模板提供一致的布局，模板位于/layout目录。
这里有3个类型的布局，简单，列表，与局部。每个模板关键位拿一些内容作为输入，并根据模板定义的方式转换。

## 简单模板
一个简单模板呈现一个简单网页，关于页面就是案例

## 列表模板
一个列表模板呈现一组相关内容，它可以是所有最近内容，或者所有属于某一类别的内容。
首页模板是特殊的列表模板，hugo假设首页模板与你提供的所有内容有关联。

## 局部模板
局部模板使用简码可以注入到任何其他模板类型。当你在网站多次重复呈现同一内容的时候他是非常有用的。头部和脚部就是这样的，它们是很好的局部模板。

# OK，现在开始
所以，现在你已经基本了解hugo。使用hugo创建新的网站。hugo提供一个命令新建网站。我们将使用这个命令建立好网站的基本。它将会创建基本网站的基本骨骼与给你一个基础配置文件。
--
$ hugo new site ~/zeo
$ cd ~/zeo
$ ls -l
total 28
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:13 archetypes
-rw-r--r-- 1 yash hogwarts   82 Feb 11 11:13 config.toml
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:13 content
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:13 data
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:13 layouts
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:13 static
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:13 themes
--
 **注意：** 我将会使用YAML格式的配置文件，hugo默认使用TOML格式。

这是目录结构的小描述：
..* **archetypes：** 包含你网站内容类型的预定义摘要格式。它促使了网站的所有内容的一致性。
..* **content：** content目录包含将会转换为提供给用户的HTML内容的Markdown文件。
..* **data：**  来自hugo文档
> 当你生成你的网站的时候会，data文件夹里面保存着额外的数据给hugo使用。数据文件不是用来产生独立页面；准确来说，它们是内容文件的补充。在摘要的控制域里它可以扩展内容的功能。或许，你像在模板显示大量数据集（看下面的案例）在这两个任务里，将这些资料外包到文件里是非常好的主意。

..* **layouts：** layout文件夹保存着所有模板文件。它们是用来呈现内容的文件。
..* **static：** static文件夹将会包含所有静态资源，比如css，js与图像。
..* **themes：** themes文件夹里面储存着我们的主题。

我们将会编辑config.yaml文件编辑网站的基本配置。
--
$ vim config.yaml
baseURL: /
title: "My First Blog"
defaultContentLanguage: en
languages:
  en:
    lang: en
    languageName: English
    weight: 1
MetaDataFormat: "yaml"
--

现在，当你运行你的网站，hugo将会显示错误。这很正常，因为我们的layout与主题目录还是空的。
--
$ hugo --verbose
INFO 2018/02/11 11:20:59 Using config file: /home/yash/zeo/config.yaml
Building sites … INFO 2018/02/11 11:20:59 syncing static files to /home/yash/zeo/public/
WARN 2018/02/11 11:20:59 No translation bundle found for default language "en"
WARN 2018/02/11 11:20:59 Translation func for language en not found, use default.
WARN 2018/02/11 11:20:59 i18n not initialized, check that you have language file (in i18n) that matches the site language or the default language.
WARN 2018/02/11 11:20:59 [en] Unable to locate layout for "taxonomyTerm":
...
...
--
这个指令将会创建一个public/目录。这个目录将会保存你网站生成相关的HTML文件。也保存所有静态数据在里面。
让我们看看public文件夹。
--
$ ls -l public/
total 16
drwxr-xr-x 2 yash hogwarts 4096 Feb  11 11:22 categories
-rw-r--r-- 1 yash hogwarts  400 Feb  11 11:25 index.xml
-rw-r--r-- 1 yash hogwarts  383 Feb  11 11:25 sitemap.xml
drwxr-xr-x 2 yash hogwarts 4096 Feb  11 11:22 tags
--
hugo生成一些XML文件，但是没有HTML文件，这是因为我们没有创建任何内容在我们的content目录造成的。
在这点上，你已经有你自己的网站，剩下就是添加一些主题和内容到你的网站。

# 创建一个新主题
hugo没有装着默认主题，hugo网站哪里有非常多的主题可以使用。hugo装着一些命令来创建主题。

在这个教程，我们将会创建一个叫做zeo的主题。入前所述，我的目的是向你展示如何使用hugo的功能使你的markdown内容填充你的HTML文件，我不会讲关于CSS，因此主题使很难看的，但又功能。

让我们创建一个主题的基本骨架。它将会创建主题的目录结构与空文件，你会填写它。
--
# run it from the root of your site
$ hugo new theme zeo
$ ls -l themes/zeo/
total 20
drwxr-xr-x 2 yash hogwarts 4096 Feb 11 11:30 archetypes
drwxr-xr-x 4 yash hogwarts 4096 Feb 11 11:30 layouts
-rw-r--r-- 1 yash hogwarts 1081 Feb 11 11:30 LICENSE.md
drwxr-xr-x 4 yash hogwarts 4096 Feb 11 11:30 static
-rw-r--r-- 1 yash hogwarts  432 Feb 11 11:30 theme.toml
--
如果你要对世界发现你的主题，你需要填写LICENSE.md与theme.toml文件。

现在我们编辑config.yaml文件来使用我们的主题。
--
$ vim config.yaml
theme: "zeo"
--

现在里面又一个空主题，让我们编译网站。
--
$ hugo --verbose
INFO 2018/02/11 11:34:14 Using config file: /home/yash/zeo/config.yaml
Building sites … INFO 2018/02/11 11:34:14 syncing static files to /home/yash/zeo/public/
WARN 2018/02/11 11:34:14 No translation bundle found for default language "en"
WARN 2018/02/11 11:34:14 Translation func for language en not found, use default.
WARN 2018/02/11 11:34:14 i18n not initialized, check that you have language file (in i18n) that matches the site language or the default language.

                   | EN
+------------------+----+
  Pages            |  3
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  0
  Processed images |  0
  Aliases          |  0
  Sitemaps         |  1
  Cleaned          |  0

Total in 12 ms
--
在这个任务里，这些警告是无伤大雅的，我们只做一个英文版本的网站。

在生成网站的过程中hugo只做两件事。使用模板转换所有内容文件到HTML，与复制静态资源到你的网站，hugo不会转换静态文件，只原样复制。

# 周期
平常的开发hugo主题的开发周期是：
..* 删除 /public 文件夹
..* 运行内置web服务器与打开浏览器看你的网站
..* 更改主题.
..* 在浏览器查看更改
..* 重复第三步

必须删除public目录，因为hugo不会去删除这个文件夹过时的文件，因此，就数据可能会干扰你的工作流。

在版本控制软件的帮助下跟踪主题中的改变也是一个好主意。我更喜欢Git。你可以根据自己的喜好使用其他的。

# 在浏览器运行你的网站
在开发hugo主题的过程中，hugo内置web服务器给与极大的帮助。它还具有监视与重载功能，它监视文件中的改变并相应地重新加载网页.
运行 
```javascript
hugo server 
```
命令。

现在在浏览器打开http://localhost:1313，默认，hugo不会显示任何内容。因为在public目录没有找到任何HTML文件。

这个指令带 --watch 选项加载web服务器：
--
$ hugo server --watch --verbose
...
...
                   | EN
+------------------+----+
  Pages            |  4
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  0
  Processed images |  0
  Aliases          |  0
  Sitemaps         |  1
  Cleaned          |  0

Total in 11 ms
...
...
--

# 更新首页模板
在/layout文件夹，hugo查找下面的目录，搜索index.html页面。
..* index.html
..* _default/list.html
..* _default/single.html
