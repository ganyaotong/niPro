---
title: "在研究macOS平台上的swift代码的时候修复Bug,有助于OAS"
date: 2018-09-11T16:19:59+08:00
description: ""
Tags: [""]
Categories: ["软件开发"]
draft: false
---

记录一下，昨天晚上在查看别人的Swift4代码的时候发现bug，通过对这个bug研究，发现代码不完整，通过编写一部分代码来与这段代码拼接，成功解决这个Bug，我们非常高兴，这段代码在swift4中起到跨进程调用的作用，这个调用功能适合现在macOS版本的OAS，有了这段代码，OAS在macOS平台上的技术可行性提高。对我们来说非常重要。

![Bug][bug]
配图：Bug

[bug]: ../../image/2018-09-11/bug.jpg "BUG"