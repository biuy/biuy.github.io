---
layout: post
title:  "GitHub Pages博客添加全局搜索"
date:   2019-01-25
excerpt: "类似于spotlight的搜索"
image: https://ws2.sinaimg.cn/large/006tNc79gy1fzffs2jymaj31hc0u0jys.jpg
tag:
- web
- blog
---

# GitHub Pages博客添加全局搜索

转载来源：[https://github.com/androiddevelop/jekyll-search](https://github.com/androiddevelop/jekyll-search)

## 加入步骤

#### 1. 在以上网址下载源码

#### 2. 将search目录放至于博客根目录下，其中search目录结构如下:

```shell
 search
 ├── cb-footer-add.html
 ├── cb-search.json
 ├── css
 │   └── cb-search.css
 ├── img
 │   ├── cb-close.png
 │   └── cb-search.png
 └── js
     ├── bootstrap3-typeahead.min.js
     └── cb-search.js
```

#### 3. 在 `_include/footer.html` 中的 `</footer>` 后加入 `cb-footer-add.html` 中的内容即可

总之在一个所有网页都会引用的html文件能够加入 `cb-footer-add.html` 中的代码就可以。

## 注意事项

1. 需要事先引入**jquery**与**bootstrap3(js与css文件)**框架，如果没有的话，操作如下:

在`_include/head.html` 中引入以下代码:

```html
<link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">
```

在`_include/footer.html` 中引入以下代码:

```html
<!-- jQuery -->
<script src="//cdn.bootcss.com/jquery/2.2.2/jquery.min.js"></script>

<!-- Bootstrap Core JavaScript -->
<script src="//cdn.bootcss.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
```

**bootstrap3-typeahead.min.js 的引入必须在jquery.min.js引入之后**

2. 更多查看原文链接

## 遇到的Bug

主要就是jQuery和bootstrap的问题，有些博客的样式在加入这些文件后会发生改变。

更普遍的选择是安装插件`Simple-Jekyll-Search`也是可以的。

但是写这样一个自定义的全局搜索也是比较容易的，本文的原理如下

[https://www.codeboy.me/2015/07/11/jekyll-search/](https://www.codeboy.me/2015/07/11/jekyll-search/)

操作步骤大致为：

```
① 服务端生成文章索引
② 浏览器获取文章索引
③ 页面交互以及按键控制
```

