---
layout: post
title:  "GitHub Pages博客添加Gitalk评论"
date:   2019-01-22
excerpt: "本文即Gitalk示例"
image: https://ws2.sinaimg.cn/large/006tNc79gy1fzffs2jymaj31hc0u0jys.jpg
tag:
- gitalk
- web
- blog
---



# GitHub Pages博客添加Gitalk评论

> 由于出了很多bug，我们严格根据官方的说法来配置

官方中文文档：[https://github.com/gitalk/gitalk/blob/master/readme-cn.md](https://github.com/gitalk/gitalk/blob/master/readme-cn.md)

安装方式非常简洁

## 1. 安装

- 直接引入

```html
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
```

### 关于在哪里引入

严格将`css`文件加入到`head`标签下

> 在这个Blog的主题下，是在一个head.html文件中加入这句

```html
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
```

在要添加评论的文章页面的`body`标签下加入`js`文件

> 在这个Blog的主题下，文章的页面即post.html

```html
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
```

## 2. 使用

添加一个容器：

```html
<div id="gitalk-container"></div>
```

将这句话放在添加评论的位置

> 注意每个容器会生成不同的issue
>
> 如果每个文章都生成一个容器，那么每个文章对应着不同的issue

用下面的 Javascript 代码来生成 gitalk 插件：

```javascript
var gitalk = new Gitalk({
  clientID: 'GitHub Application Client ID',
  clientSecret: 'GitHub Application Client Secret',
  repo: 'GitHub 仓库名',
  owner: 'GitHub 账户名',
  admin: ['GitHub 账户名数组'],
  id: location.href,      // Ensure uniqueness and length less than 50
  distractionFreeMode: false  // Facebook-like distraction free mode
})

gitalk.render('gitalk-container')
```

需要 **GitHub Application**，如果没有 [点击这里申请](https://github.com/settings/applications/new)

- `Authorization callback URL` 填写当前使用插件页面的域名（即博客名称）。

- 申请时的两个URL即博客地址
- 申请成功后得到`Client ID` 和 `Client Secret`

## 3. 设置

- **clientID** `String`

  GitHub Application Client ID.

- **clientSecret** `String`

  GitHub Application Client Secret.

- **repo** `String`

  GitHub repository.仓库名

- **owner** `String`

  GitHub repository 所有者，可以是个人或者组织。

- **admin** `Array`

  GitHub repository 的所有者和合作者 (对这个 repository 有写权限的用户)。

- **id** `String`

  Default: `location.href`.

  页面的唯一标识。长度必须小于50。

更多设置查看官方文档[https://github.com/gitalk/gitalk/blob/master/readme-cn.md](https://github.com/gitalk/gitalk/blob/master/readme-cn.md)

> 注意在Github仓库Setting里必须勾选issues

## 4. BUG

### 1. Gitalk加载不出来

   **首先检查**

   ```javascript
   var gitalk = new Gitalk({
     clientID: 'GitHub Application Client ID',
     clientSecret: 'GitHub Application Client Secret',
     repo: 'GitHub 仓库名',
     owner: 'GitHub 账户名',
     admin: ['GitHub 账户名数组'],
     id: location.href,      // Ensure uniqueness and length less than 50
     distractionFreeMode: false  // Facebook-like distraction free mode
   })
   
   gitalk.render('gitalk-container')
   ```

   - `符号的位置对不对？

   - 仓库名与账户名对不对？

   - **里面的注释最后全都去掉**

     后来加载的时候发现出现了这种效果：

     ```javascript
     id: location.href,      // Ensure uniqueness and length less than 50	distractionFreeMode: false  
     ```

     后面的东西全都被注释掉了！因此建议注释都去掉

   - 检查`Github Application`

     - 两个URL是博客地址吗？

------

### **2. Validation Failed(422)**

参考了一篇文章：[处理Gitalk中由于文章URL过长导致的Validation Failed(422)](https://priesttomb.github.io/日常/2018/02/12/处理Gitalk中由于文章URL过长导致的Validation-Failed(422)/)

#### 0. 找一个 js 实现的 md5

网上随便找了找，用的是这个[JavaScript-MD5](https://github.com/blueimp/JavaScript-MD5)

#### 1. 引入 js，对 url 加密

```html
<script src="{{ site.url }}/assets/js/md5.min.js"></script>
<script type="text/javascript">
    var gitalk = new Gitalk({
  	clientID: 'GitHub Application Client ID',
  	clientSecret: 'GitHub Application Client Secret',
  	repo: 'GitHub 仓库名',
  	owner: 'GitHub 账户名',
  	admin: ['GitHub 账户名数组'],
  	id: md5(location.href),      // Ensure uniqueness and length less than 50
  	distractionFreeMode: false  // Facebook-like distraction free mode
	})

	gitalk.render('gitalk-container')
</script>
```

#### 2. 测试验证

提交后打开之前有问题的文章，发现已经可以正常创建 issue 了

![](https://ws3.sinaimg.cn/large/006tNc79gy1fzfg8uz3enj31ay0u0acx.jpg)