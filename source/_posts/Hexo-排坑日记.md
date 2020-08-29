---
title: Hexo 排坑日记（持续更新）
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008232543535740.jpg
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008232543535740.jpg
tags:
  - Hexo
excerpt: 在此记录我在搭建 Hexo 博客中所遇到的坑以及解决办法。
abbrlink: '7487'
date: 2020-08-13 14:13:28
categories: 折腾派
---

{% note success %}

在此记录我在搭建 Hexo 博客中所遇到的坑以及解决办法。

{% endnote %}

## 前言

曾经年少被 “10 分钟搭建属于自己的个人博客” 的教程（ ~~忽悠~~ ）所鼓舞。风风火火地搭建好基于 Hexo 的个人博客，可谁知接下来等着自己的是各种出乎意料的 Bug。

## YAML 语法错误

-  **`YAML Exception: bad indentation of a mapping entry at line xxx`  错误提示**

  根据 [Hexo 官方文档](https://hexo.io/zh-cn/docs/troubleshooting.html) 说明：

  > 如果 YAML 字符串中包含冒号（`:`）的话，请加上引号。
  >
  > 请确认您使用空格进行缩进（Soft tab），**并确认冒号后有加上一个空格**。

  在对应的提示位置加上空格或者引号即可。YAML 拥有严格的语法标准，在某些地方多一个或少一个空格都会导致编译报错。

  **But**！在一次修改代码友链代码的过程中，提示友链的某个位置语法错误。在检查提示位置后未发现任何缺少引号或空格的错误。随后将此段代码放入 YAML 编辑器中并未提示错误。进一步检查中发现是**之前一段代码**出现了语法错误（多了一个引号）。

  也就是说，如果报错 `YAML Exception: bad indentation of a mapping entry at line xxx`，在检查此位置没有语法错误之后，应该对其余位置的代码进行语法检查。
  
  {% note danger %}
  
  **注**：所以建议在修改无果的情况下，将所有代码都复制到 [YAML、YML在线编辑(校验)器](http://www.bejson.com/validators/yaml_editor/) 中进行语法检查，而不是只将某一段代码带入检查。
  
  {% endnote %}

## 其他问题

-  **webp 格式图片无法在 Safari 浏览器显示**

  原因：webp 是谷歌研发的，属于商业纠葛。
  
  解决：为了全平台都能正常显示，并且不想额外折腾 CCS 代码，只好乖乖在博客中不使用 webp 格式的压缩图片。

