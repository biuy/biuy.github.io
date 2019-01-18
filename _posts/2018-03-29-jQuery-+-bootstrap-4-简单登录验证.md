---
layout: post
title:  "jQuery + bootstrap 4 简单登录验证"
date:   2018-03-29
excerpt: "内有源码，就不传github了"
image: https://ws2.sinaimg.cn/large/006tNc79ly1fzasor0s0sj30cz0dkwf1.jpg
tag:
- web
- 前端
- JavaScript
---

# jQuery + bootstrap 4 简单登录验证

迁移旧文

~~还是有点少女~~

早期实现的第一个原创模板，后面的很多作品都是建立在它的源码之上

------

简单的表单验证bootstrap4自带的js文件就可以实现哦

bootstrap.js自带的表单验证的效果：![https://ws2.sinaimg.cn/large/006tNc79ly1fzasor0s0sj30cz0dkwf1.jpg](https://ws2.sinaimg.cn/large/006tNc79ly1fzasor0s0sj30cz0dkwf1.jpg)

![https://ws1.sinaimg.cn/large/006tNc79ly1fzasp22abxj30el0e9q3l.jpg](https://ws1.sinaimg.cn/large/006tNc79ly1fzasp22abxj30el0e9q3l.jpg)

![https://ws4.sinaimg.cn/large/006tNc79ly1fzaspd6okvj30d70dwmxs.jpg](https://ws4.sinaimg.cn/large/006tNc79ly1fzaspd6okvj30d70dwmxs.jpg)

![https://ws3.sinaimg.cn/large/006tNc79ly1fzaspll437j30cr0dr0ta.jpg](https://ws3.sinaimg.cn/large/006tNc79ly1fzaspll437j30cr0dr0ta.jpg)

下面贴html代码
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="logo" href="images/logo.png">

    <title>Signin</title>
     
    <!-- Bootstrap core CSS -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <link href="css/messenger.css" rel="stylesheet">    
    <link href="css/messenger-theme-future.css" rel="stylesheet">  
    <link href="css/signin.css" rel="stylesheet">
    <link href="plugins/font-awesome-4.7.0/css/font-awesome.min.css" rel="stylesheet">

  </head>

  <body class="text-center">
​    <script type="text/javascript" src="js/jquery-3.3.1.slim.min.js"></script>
​    <script type="text/javascript" src="js/signin.js"></script>
​    <script type="text/javascript" src="js/bootstrap.min.js"></script>
​    <script type="text/javascript" src="js/messenger.min.js"></script>

    <div class="container">
      <form class="form-signin" action="passed.html">
        <img class="mb-4" src="images/logo.png" alt="" width="180" height="72">
        <h2 class="form-signin-heading">Please sign in</h2>
        <label for="inputEmail" class="sr-only">Email address</label>
        <input type="email" id="inputEmail" class="form-control" placeholder="Email address" required autofocus>
        <label for="inputPassword" class="sr-only">Password</label>
        <input type="password" id="inputPassword" class="form-control" placeholder="Password" required>
        <div class="checkbox">
          <label>
            <input type="checkbox" value="remember-me"> Remember me
          </label>
        </div>
        <button class="btn btn-lg btn-primary btn-block" id="check" type="submit">Sign in</button>
      </form>
     
    </div> 
  </body>
</html>
```
非常简单而且好看呢。这个pass.html是登录成功后跳转的页面就不放代码了随便写写。

这个图片感觉也可以换成更合适的fontawesome的home图标
```html
<i class="fa fa-home"></i>
```

贴一下奇怪的signin.css……大家可以自己根据不带这个文件时，原有的样子设计（就不会绿了）：
```css
body {
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #eee;
}

.form-signin {
  max-width: 330px;
  padding: 15px;
  margin: 100px auto;
}
.form-signin .form-signin-heading,
.form-signin .checkbox {
  margin-bottom: 10px;
}
.form-signin .checkbox {
  font-weight: normal;
  font-size: 15px;
}
.form-signin .form-control {
  position: relative;
  height: auto;
  -webkit-box-sizing: border-box;
​     -moz-box-sizing: border-box;
​          box-sizing: border-box;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
.btn-primary {
  background-color: #9BCD9B;
  border-color: #9BCD9B;
}
.btn-primary:hover {
  background-color: #7CCD7C;
  border-color: #7CCD7C;
}
```
虽然写颜色的rgb值时随便写写用了大写，不过实际最好用小写哦。。



最重要的还是登录名的验证。

我们的目的是实现最简单的登录验证。之前查了很多资料也没找到简单的登录认证实现，后来用了一种比较粗暴的形式，很简单的几行，signin.js:
```javascript
$(function() {
	$("#check").click (function() {
		if($('#inputEmail').val()=='atbybiu@163.com'&& $('#inputPassword').val()=='123'){
	 		alert('登陆成功！');

			return true;
	 	}else {
	 		$('#inputEmail').val('');//清空输入框
	 		$('#inputPassword').val('');
	 		alert('登陆失败！');
	 
	 		return false;
	 	}
	 });
});
```
虽然简单粗暴不过能做到最简单的认证：

![](https://ws1.sinaimg.cn/large/006tNc79ly1fzasx02474j30ff0jugme.jpg)

点击确定后跳转的页面:

![](https://ws3.sinaimg.cn/large/006tNc79ly1fzasxdb7w0j30e60bhmxj.jpg)

如果用户名或密码的字符不对:

![](https://ws3.sinaimg.cn/large/006tNc79ly1fzasxo2osoj30fx0j7752.jpg)

点击确定后输入框全部清空：

![](https://ws1.sinaimg.cn/large/006tNc79ly1fzasy00rttj30d80erjru.jpg)



~~我怀疑我曾经对代码的绝对完美有一定的幻想~~

作者：atbybiu 
来源：CSDN 
原文：https://blog.csdn.net/atbybiu/article/details/79749527 
版权声明：本文为博主原创文章，转载请附上博文链接！

**如果能再顺手点个关注就更好了~**