---
title: Hexo's Fluid 主题私人定制（持续更新）
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200813020009.jpg
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200813020009.jpg
tags:
  - Hexo
  - Fluid
abbrlink: 40222
date: 2020-08-08 11:55:36
excerpt: 本文介绍如何修改 Fluid 主题配置文件，以实现版权声明优化、显示运行时间、关于页添加评论、自定义字体等功能。
categories: 折腾派
---

{% note success %}

运行环境：

[Fluid](https://github.com/fluid-dev/hexo-theme-fluid/releases) version：1.8.1

[Hexo](https://hexo.io/zh-cn/) version：4.2.1

{% endnote %}

{% note danger %}

注：随着主题版本升级，主题文件内容可能有变动，请谨慎参考。

{% endnote %}

# 前言

在茫茫 Hexo 主题中，一眼选中了 Material Design 风格的 [Fluid 主题](https://github.com/fluid-dev/hexo-theme-fluid)。老实说基本功能已经完全够用了，可以骨子里那股折腾劲儿又来了，看着自己的网站总觉得哪儿哪儿不顺心。于是有了此贴，专门用来记录笔者自定义 Fluid 主题的过程，以便日后查阅，同时也供相同主题版本的朋友参考。

# 版权声明优化

{% note primary %}

目的：修改文章页底部版权声明内容，实现显示 “本文作者”、“本文地址”、“版权声明” 的内容。

{% endnote %}

目前 Fluid 版本（v1.8.1）中文章末尾版权声明仅设置显示为：

![Snipaste_2020-08-08_12-08-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008120027886.png)

而看到许多博客文章末尾都有完整的文章信息，例如“本文作者”，“本文地址”，“版权声明”等。

在 Next 等主题中自带文章末尾可以添加“文章链接”，只需要在 `_config.yml` 中 `enable` 相应模块即可，效果如下图所示。

![Snipaste_2020-08-08_13-41-30](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812000800.png)

而在 Fluid 主题中则需要用户对相应配置文件进行修改。

在网上搜寻方法时发现 [七夏浅笑](https://github.com/julydate) 针对 Fluid 主题修改的版本实现了版权声明处的文章链接显示。因为都是使用 Fluid 主题，所以想着对方应该能够解答自己的疑惑所以通过邮件进行了联系。很快得到了大佬答复：

![Snipaste_2020-08-08_14-04-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812002839.png)

根据大佬的提示（还贴心地标出代码位置，小姐姐很 nice 了），并结合网上相应对版权声明修改的资料进行了操作。

## 步骤

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

## 错误做法

**修改主题配置文件 `config.yml`**

一开始笔者以为只需要简单的修改主题配置文件 `_config.yml` 里面版权声明处，但在“本文链接”无法正常生成链接。即使在“本文链接”后面加上代码仍然失败。

```
<a href="<%- full_url_for(page.path) %>"><%- full_url_for(page.path) %></a>
```

![微信图片_20200807203239](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003717.png)

`hexo s` `hexo g` 生成结果如下图：

![Snipaste_2020-08-08_13-57-16](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003237.png)

**单纯复制修改主题的代码**

前面谈到有位小姐姐修改的 [Fluid主题]() 能实现我的目标，我寻思着直接找她改动的代码不就行了吗。于是直接就把相应位置的代码给 copy 了过来，结果造成了页面错误。

![Snipaste_2020-08-07_23-39-01](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812003422.png)

最后进行修改调试，才恢复正常。

# 添加运行时间

<p class="note note-primary">目的：在站点页面页脚处实时显示网站运行时间 & 自定义页脚信息。</p>

## 步骤

1. 打开文件夹 `\themes\fluid\layout\_partial` 下的 `footer.ejs` 文件。

2. 在任意处添加如下代码：

   ```
   <div>
     <span id="timeDate">载入天数...</span>
     <span id="times">载入时分秒...</span>
     <script>
     var now = new Date();
     function createtime(){
         var grt= new Date("07/02/2020 00:00:00");//此处修改你的建站时间或者网站上线时间
         now.setTime(now.getTime()+250);
         days = (now - grt ) / 1000 / 60 / 60 / 24;
         dnum = Math.floor(days);
         hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum);
         hnum = Math.floor(hours);
         if(String(hnum).length ==1 ){
             hnum = "0" + hnum;
         }
         minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
         mnum = Math.floor(minutes);
         if(String(mnum).length ==1 ){
                   mnum = "0" + mnum;
         }
         seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
         snum = Math.round(seconds);
         if(String(snum).length ==1 ){
                   snum = "0" + snum;
         }
         document.getElementById("timeDate").innerHTML = "🚀 for&nbsp"+dnum+"&nbspdays";  //此次自定义显示内容
         document.getElementById("times").innerHTML = hnum + "&nbsphr&nbsp" + mnum + "&nbspmin&nbsp" + snum + "&nbspsec";
     }  //此次自定义显示内容
     setInterval("createtime()",250);
     </script>
   </div>
   ```

3. 在标注处修改你自己的建站时间，同时自定义显示内容。例如笔者自定义的内容就是 “🚀 for 55 days 18 hr 09 min 37 sec”。

4. （可选）修改字体样式和大小：![Snipaste_2020-08-26_18-17-54](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200826181808.png)

5. 修改页脚信息，只需将 `footer.ejs` 中对应位置改为你想显示的内容即可。例如笔者的代码如下：

   ```
     <div class="text-center py-1">   
       <div>
         <span>Copyright © 2020</span></a>
         <a href="https://erenspace.cool/" target="_blank" rel="nofollow noopener">
          <span>Eren‘s Spaceship</span></a>    <br>
       </div>
   ```

6. 实现效果：

![Snipaste_2020-08-26_18-14-44](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200826181457.png)

# 关于页添加评论

{% note primary %}

目的：在关于页添加评论功能，以实现更好的博客互动。

本文以 Valine 评论系统作为示范。

{% endnote %}

## 步骤

1. 打开之前自行创建的关于页 `.md` 文件：![Snipaste_2020-08-26_18-43-02](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200826184334.png)

2. 在你想要的位置添加 Valine 评论系统的代码。你可在别处 CCS 文件找到你对应的 Valine 评论系统代码，也可以直接复制以下代码（复制用纯文本，这样才是 HTML 代码，否则系统会以为是文章内容代码）。**注意将 app_id 和 app_key 换成你自己的。**

   ```
    <div id="vcomments"></div>
     <script type="text/javascript">
       function loadValine() {
         addScript('https://cdn.staticfile.org/valine/1.4.14/Valine.min.js', function () {
           new Valine({
             el: "#vcomments",
             app_id: "填写在 Leancloud 中的数据",
             app_key: "填写在 Leancloud 中的数据",
             placeholder: "留下点什么叭... ᶘ ᵒᴥᵒᶅ（自行修改）",
             path: window.location.pathname,
             avatar: "monsterid",
             meta: ["nick","mail","link"],
             pageSize: "10",
             lang: "zh-CN",
             highlight: false,
             recordIP: false,
             serverURLs: "",
           });
         });
       }
       createObserver(loadValine, 'vcomments');
     </script>
     <noscript>Please enable JavaScript to view the <a href="https://valine.js.org" target="_blank" rel="nofollow noopener noopener">comments
         powered by Valine.</a></noscript>
   ```

   实现效果参考：https://erenship.com/about/

# 自定义字体

{% note primary %}

目的：在网站中引入自定义字体。

本教程以引入思源宋体为例。

{% endnote %}

字体，作为影响网页美观的重要因素。我们常常需要个性化字体来突显网站的风格。但由于中文字体不同于英文字体只需覆盖 26 个字母 ，中文字体包由于包含大量中文字库，其文件大小通常有几兆甚至十几兆。这给网页浏览来带极大的流量负担，拖慢加载速度，影响用户体验。

在 [Fluid 官方文档](https://hexo.fluid-dev.com/docs/guide/#%E5%85%A8%E5%B1%80%E5%AD%97%E4%BD%93) 中也建议使用系统自带字体。

> 需要注意：
>
> - 最好使用系统自带的字体，否则需要通过[自定义功能](https://hexo.fluid-dev.com/docs/guide/#自定义-js-css-html)额外引入 `@font-face`，字体一般较大，不建议引入；
> - 应当至少添加一个通用的字体族名（如 serif，具体见上方链接文章）。

但是，这怎么能止住我们追求美的脚步呢？

在此我们引出今天的主角： [字蛛（font-spider）](http://font-spider.org/)

> 字蛛是一个中文字体压缩器
>
> 让网页自由引入中文字体成为可能

## 步骤

**添加自定义字体**

1. 先创建自定义字体所需要的一些文件：

   在博客根目录 (非theme目录) `source` 文件夹下创建 `fonts` 文件夹（用于存放字体文件）；
   在 `\themes\fluid\source\css` 下新建 `custom.css` 文件;
   在 Fluid 主题对应的 `_config.yml` 找到`custom_css:` 在后面添加 `/css/custom.css`。

2. 下载目标字体，推荐网站：[google-webfonts-helper](https://google-webfonts-helper.herokuapp.com/)

3. 进行声明和设置字体：

   将下载好的文件解压到夹刚刚创建的 `fonts` 文件夹中，再将网页自动生成的 @font-face 定义放到 `/css/custom.css` 中。

   ![Snipaste_2020-08-26_19-53-36](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/2020082620020588.png)

4. 将字体英文名称添加到 `\themes\fluid\source\css\variables\` 目录下的 `base.styl` 文件中，具体位置为 `font-family:` 。例如笔者引入的字体位思源宋体，其英文名称为 “Noto Serif SC”。![Snipaste_2020-08-26_20-01-37](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200826200147.png)

至此，我们就完成了自定义字体引入。你可以执行部署命令来检查是否成功引入。由于未对字体压缩，新字体加载应该会比较慢。

**注：**如果检查发现没有引入成功，请检查`fluid_config.yml` 中如下部分是否进行改动。可以添加引入字体英文名称。例如笔者所引入字体的英文名称： ` font_family:  Source Serif Pro, Noto Serif SC`，再进行重新部署检查。

```
font:  # 主题字体配置
  font_size: 16px                   # 全局字号
  font_family:                      # 全局 Georgia, sans-serif
  code_font_size: 100%              # 代码的字号
```

**字体压缩**

1. 安装字蛛。

   ```
   npm install font-spider -g
   ```

2. 生成新的字体库。

   ```
   font-spider  <博客文件夹>\public\index.html
   ```

   此处 <博客文件夹> 是对应 `index.html` 文件所在的目录。例如笔者此处所执行的代码为：

   ```
   font-spider  D:\MyBlog\public\index.html
   ```

至此，我们便完成了引入字体 + 字体压缩的所有工作。你可以执行部署命令来检查现在网页加载是否有较大提升。

​         

{% note danger %}

生命在于折腾。

{% endnote %}

# 参考资料

- [Fluid配置文档](https://hexo.fluid-dev.com/docs/guide/#%E4%B8%BB%E9%A2%98%E7%AE%80%E4%BB%8B)
- [Hexo Fluid 主题 UI 修改版](https://github.com/qixa/hexo-theme-fluid-mod)
- [Hexo Fluid主题 添加自定义字体](https://largesse.12306.recipes/posts/3c2a5351.html)
- [我是如何加速博客的](https://blog.jalenchuh.cn/posts/how-i-make-blog-faster/)
- [ttf 字体压缩](https://lruihao.cn/posts/web-font.html)

