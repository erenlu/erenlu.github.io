---
title: about
date: 2020-07-02 08:24:44
layout: about
comments: true
---
> 偏爱科技且执念于人文，
>
> 力求于在全然拥抱当下的同时探索认知的边界。
>

## 关于Eren

- 生活：97 生人，热爱互联网与科技，亦喜欢阅读各类杂书；
- 学业：目前是一名学生，主攻嵌入式软件设计 &  室内定位相关算法~~（如果入门也称得上是研究的话）~~；
- 理想：站在人文与科技的交汇点肆意造作。

## 何以存在

独立博客，算是我对当下愈加封闭的互联网环境的一种反叛。

我会尽管写下去，去记录生活。在此借一位前辈所言：

> *“坚持下来的都足以证明你是一个与众不同的人，也许在途中会发现很多与你志同道合的人！”*

## 更新日志

- 2020-8-12

  > 弃用本地图库，启用 [GitHub](https://github.com/) + [jsDelivr](https://www.jsdelivr.com/) + [PicGo](https://github.com/Molunerfinn/picgo/releases) 搭建图床，提升站点图片加载速度。
  
- 2020-8-10

  > 主题优化（英文字体更改为 Georgia，改善代码大小，新增页脚信息，优化版权声明）；
  >
  > 修改 “关于页” 排版，新增 “更新日志”。

- 2020-8-7

  > 拥有了第一条友链 [@vincentqin](https://vincentqin.tech/)；
  >
  > 使用 [Vercel](https://vercel.com/) 为站点加速。
  >
  
- 2020-7-12  

  > 使用 [Valine](https://valine.js.org/) 作为博客评论系统。

- 2020-7-2    

  > 基于 [Hexo 框架](https://hexo.io/zh-cn/) + [Fluid 主题](https://github.com/fluid-dev/hexo-theme-fluid) 创建 Eren's Spaceship, 并成功启航。

## 写在最后

感谢你花时间阅读我的博客，欢迎与我留言交流。祝好！



​                           

 <div id="vcomments"></div>
  <script type="text/javascript">
    function loadValine() {
      addScript('https://cdn.staticfile.org/valine/1.4.14/Valine.min.js', function () {
        new Valine({
          el: "#vcomments",
          app_id: "UH7QfXQF6JMWo43UdP0ryQ8r-MdYXbMMI",
          app_key: "Xw3iWYWBBjFYvJr6L5b4kVQY",
          placeholder: "留下点什么叭... ᶘ ᵒᴥᵒᶅ",
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

