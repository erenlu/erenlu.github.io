---
title: Hexo 博客站点加速不完全指南
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200813012440.jpg
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200813012440.jpg
excerpt: 从 Vercel 托管到 jsDelivr CDN 加速，以及添加 Valine 评论系统...这里是加速 Hexo 博客站点不完全指南。
abbrlink: db37
date: 2020-08-12 01:48:09
categories: 折腾派
---

## 前言

在我们使用 Hexo 作为框架搭建静态博客时，存在两个主要因素影响着网站访问速度。

- GitHub 在国内访问速度不稳定

  由于我们搭建的博客是放在 GitHub 上面的，而国内用户直接访问 GitHub 网速不佳。这将是十分影响博客的用户体验。所以使用国内访问速度更快的托管服务商 Vercel （当然也可以用别的服务商，只是在国内来说 Vercel 效果较好）进行网站托管；

- 在使用本地图库引用的情况下，当博客图片资源过大时，网站图片加载会变得缓慢。


所以，我们在此主要针对以上两个问题来进行优化。

## Vercel 托管

**Vercel** 是一家提供 JamStack（静态网站）托管的平台，支持自动从 GitHub 等仓库拉取代码, 按自定义构建方式进行构建，最后把生成的静态网站进行发布; 在这基础上同时也支持自定义域名，自动申请 SSL 证书等功能。

由于国内访问 Vercel 的速度要比访问 GitHub pages快很多，所以我们只需要把站点交给 Vercel 进行托管就能大幅度提升博客的加载速度。

{% note success %}

参考教程： [使用 Vercel 加速 Hexo 静态博客访问](https://vincentqin.tech/posts/speedup-gitpage/) 

{% endnote %}

在使用 Vercel 托管之后，访客在访问博客网站时相当于是在访问 Vercel 的服务器（因为此时已经在域名服务商处处新增了对应 Vercel 站点的域名解析），而不是去访问最开始的 GitHub pages 网址。

此外，按照上述教程进行操作之后，查看 Domain 一栏是否解析生效。此时可能会出现一会儿配置失败，一会儿配置成功的情况。若出现标红提示配置失败，点击查看错误提示。**如果是提示更改为 Vercel 的 DNS 服务器，只需到你博客域名服务商，将 DNS 服务器地址更改为 Vercel 要求的地址即可。**（例如我的域名服务商是阿里云，我到官网去把阿里云官方的 DNS 服务器地址改为 Vercel 的即可。DNS 服务器地址为两个，都需要进行更改）![Snipaste_2020-08-12_02-35-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812033929.png)

**效果图**

对比站点 github.io 的原始域名，访问延迟明显减小：

![对比](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200814001334.png)

使用百度站长对网站速度进行测试：

![网站速度诊断](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812040604.png)

## jsDelivr 加速

在博客的撰写中，我们一般是直接引用本地图库资源（使用 hexo-asset-image 插件）对文章进行配图。之后再`hexo g -d` 将文章和图片资源一同部署到 GitHub 。而我们在访问这些图片资源时又需要再从 GitHub 上进行提取，而 GitHub 上的访问速度本身已经很慢，再去加载大量图片更加会拖慢网页速度。虽然我们前面已经对博客进行了 Vercel 托管，但是我们有一种更简单高效的方式对图片进行上传，那就是**利用 GitHub 图库+ jsDelivr CDN 加速+ PicGo 客户端实现图库的高效使用**。

简单说明以上三个工具的使用情形：

- GitHub 图库：用于存放我们需要使用的文章配图文件的仓库；

- jsDelivr ：用于 CDN 加速，在 Markdown 文本编辑器中直接引用 jsDelivr 的图片外链，即可快速访问图片资源；

- PicGo：用于 Markdown 写作端生成对应的 jsDelivr 图片外链，配合 Typora 编辑器时可直接拖拽图片自动生成外链使用。（**故推荐 Typora 客户端 + PicGo 客户端搭配使用，食用效果更佳！**）

  ​       

**什么是 jsDelivr ?**

> jsDelivr 是国外的一家优秀的公共CDN 服务提供商，也是首个「打通中国大陆（网宿公司运营）与海外的免费CDN 服务」。 jsDelivr 有一个十分好用的功能——它可以加速Github 仓库的文件。 我们可以借此搭建一个免费、全球访问速度超快的图床。

**什么是 PicGo ?**  

> 一个用于快速上传图片并获取图片URL链接的工具。

{% note success %}

参考教程：

- [Github+jsDelivr+PicGo 打造稳定快速、高效免费图床](https://www.itrhx.com/2019/08/01/A27-image-hosting/)
- [Typora原生集成PicGo图床工具！](https://www.cnblogs.com/hoxis/p/12470044.html) 

{% endnote %}

****

**此方法会弃用本地图库，启用 GitHub + jsDelivr + PicGo 的方式对文章进行配图操作。** 

所以可以卸载 hexo-asset-image 插件，保持 Hexo 的清爽：

```
npm remove  hexo-asset-image
```

并在配置文件_config.yml 中将此行由 true 改为 false：

```
 post_asset_folder: false
```

此时，在你每次`hexo n "title"` 的时候就不会在 _posts下面生成同名的文件夹放图片文件了，大可将需要使用的图片放在除博客文件的其他位置（所以你甚至可以把已经生成外链的图片删掉），因为照片放在专门的图片仓库，利用 jsDelivr 外链就可以直接访问，你不需要再将这些图片上传到博客的 GitHub 仓库里面去。下图红色地址就是我们直接可以访问到图片的 jsDelivr 外链地址。你可以点来试下，看看是不是我们下面这张图。 https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812034534.png

![Snipaste_2020-08-12_03-45-17](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812034534.png)



> 注：在使用 PicGo 客户端时会遇到一些问题，具体解决方法参考：[Typora原生集成PicGo图床工具！](https://www.cnblogs.com/hoxis/p/12470044.html) 

在使用图床之后，我们所用的的图片就可以不放在本地。那么图片在每次编译部署也就不用上传到网上，这缩短了编译时间。

## Valine 评论系统 （邮件提醒）

{% note success %}

参考教程：

- [LeanCloud-Valine 保姆及配置教程](https://lete114.now.sh/article/Valine-LeanCloud-Config.html#%E5%89%8D%E8%A8%80)
- [LeanCloud 因控流原因的解决办法](https://lete114.now.sh/article/da1d5c8b.html)

{% endnote %}

## 其他优化教程

{% note success %}

参考教程：

- [Hexo 基础教程(四)：功能添加与优化](https://indexmoon.com/articles/1153730074/)

  包含：添加 RSS，代码压缩，文章链接唯一化，SEO（搜索引擎优化）等内容。

{% endnote %}

## 后记

完成上述操作之后，博客的打开速度不出意外就会快很多。不过做了这么多操作，我们最核心关注的点还是应该在**博客内容质量**，以及**输出频率**上。总结下来就是内容为王，同时兼顾用户体验。

## 参考资料

- [使用 Vercel 加速 Hexo 静态博客访问](https://vincentqin.tech/posts/speedup-gitpage/)
- [Github+jsDelivr+PicGo 打造稳定快速、高效免费图床](https://www.itrhx.com/2019/08/01/A27-image-hosting/)
- [Typora原生集成PicGo图床工具！](https://www.cnblogs.com/hoxis/p/12470044.html)
- [PicGo+GitHub快速实现markdown图床](https://blog.juanertu.com/archives/adff04af.html)
- [利用 GitHub + PicGo + Typora 搭建属于自己的图床](https://juejin.im/post/6844904137407086600)
- [使用 jsDelivr CDN 加速 Github 仓库的图片，以作为博客的图床](https://blog.iljw.me/2019/05/jsdelivr-cdn-github.html)

