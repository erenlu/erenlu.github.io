---
title: Hexo's Fluid 主题的版权声明优化
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200824211.webp
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200824211.webp
tags:
  - Hexo
  - Fluid
abbrlink: 40222
date: 2020-08-08 11:55:36
excerpt: 本文通过介绍如何更改 Fluid 主题配置文件，以实现文章末尾版权声明处添加本文地址信息。
categories: 折腾派
---

> 运行环境：
>
> [Fluid](https://github.com/fluid-dev/hexo-theme-fluid/releases) version：1.8.1
>
> [Hexo](https://hexo.io/zh-cn/) version：4.2.1

# 前言

目前 Fluid 版本（v1.8.1）中文章末尾版权声明仅设置显示为：

![Snipaste_2020-08-08_12-08-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008120027886.png)

而看到许多博客文章末尾都有完整的文章信息，例如“本文作者”，“本文地址”，“版权声明”等。

在 Next 等主题中自带文章末尾可以添加“文章链接”，只需要在 `_config.yml` 中 `enable` 相应模块即可，效果如下图所示。

![Snipaste_2020-08-08_13-41-30](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812000800.png)

而在 Fluid 主题中则需要用户对相应配置文件进行修改。

# 方法

## 搜寻方法

在网上搜寻方法时发现 [七夏浅笑](https://github.com/julydate) 针对 Fluid 主题修改的版本实现了版权声明处的文章链接显示。因为都是使用 Fluid 主题，所以想着对方应该能够解答自己的疑惑所以通过邮件进行了联系。很快得到了大佬答复：

![Snipaste_2020-08-08_14-04-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812002839.png)

根据大佬的提示（还贴心地标出代码位置，小姐姐很 nice 了），并结合网上相应对版权声明修改的资料进行了操作。

## 修改步骤

1. 打开 `themes\fluid\layout` 中的 `post.ejs` 文件；

   ![Snipaste_2020-08-08_14-10-26](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812002927.png)

2. 找到以下内容进行修改；

   ![Snipaste_2020-08-08_14-15-44](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003002.png) 

   将红色部分替换为以下代码：

   ```
       <% if(theme.post.copyright.enable && theme.post.copyright.content && page.copyright !== false) { %><p class="note note-warning">
   	<strong>本文作者: </strong><a href="<%- url_for() %>"><%- theme.about.name || config.author || config.title %></a> <br>
   	<strong>本文链接: </strong><a href="<%- full_url_for(page.path) %>"><%- 		  full_url_for(page.path) %></a> <br>
   	<strong>版权声明: </strong><%- theme.post.copyright.content %>
       </p>
       <% } %>
   ```

   替换结果：

   ![Snipaste_2020-08-08_14-18-01](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202003812003531.png)

3.  保存文件，cmd  `hexo clean` `hexo s` 查看效果；

   实现目标：版权声明处添加“本文链接”并能自动生成文章链接。

   ![Snipaste_2020-08-08_14-22-34](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008120403506.png)

4. cmd  `hexo g -d`  部署博客。Bingo！

## 修改 Tag 颜色

1. 根据 [官方文档 ](https://hexo.fluid-dev.com/docs/guide/#tag-%E6%8F%92%E4%BB%B6)了解到 Tag 相应的语法；

   ![Snipaste_2020-08-08_14-48-24](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008120403517.png)

2. 如果要修改 Tag 颜色只需要修改 `<p class="note note-warning">` 即可。

# 错误做法

## 修改主题配置文件 `config.yml`

一开始笔者以为只需要简单的修改主题配置文件 `_config.yml` 里面版权声明处，但在“本文链接”无法正常生成链接。即使在“本文链接”后面加上代码仍然失败。

```
<a href="<%- full_url_for(page.path) %>"><%- full_url_for(page.path) %></a>
```

![微信图片_20200807203239](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003717.png)

`hexo s` `hexo g` 生成结果如下图：

![Snipaste_2020-08-08_13-57-16](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003237.png)

## 单纯复制修改主题的代码

前面谈到有位小姐姐修改的 [Fluid主题]() 能实现我的目标，我寻思着直接找她改动的代码不就行了吗。于是直接就把相应位置的代码给 copy 了过来，结果造成了页面错误。

![Snipaste_2020-08-07_23-39-01](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003422.png)

最后进行修改调试，才恢复正常。

# 总结

第一次尝试对 HTML 文件进行修改，因为之前没有接触过 HTML 的相关知识，所以初次看代码有些茫然。不过好在看英文单词 `copyright` 还是能看懂，以此作为切入点瞎搞竟然搞成功了。

由于笔者能力有限，并没有对许多细节进行深究，文章难免有疏漏之处。欢迎大家与我交流沟通！

​     

> 生命在于折腾。

# 参考资料

- [Fluid配置文档](https://hexo.fluid-dev.com/docs/guide/#%E4%B8%BB%E9%A2%98%E7%AE%80%E4%BB%8B)
- [Hexo Fluid 主题 UI 修改版](https://github.com/qixa/hexo-theme-fluid-mod)

