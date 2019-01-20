---
layout: post
title:  "Android Studio SDK 下载"
date:   2019-01-20
excerpt: "互联网大环境"
tag:
- Android
- 笔记
---

# Android Studio SDK 下载

### 没有VPN又没有钱

#### 1. 打开 Prefernces 设为No proxy

![](https://ws2.sinaimg.cn/large/006tNc79ly1fzd59y0n7bj318e0u0jz4.jpg)

“Apply”

#### 2. 进入网站[http://ping.chinaz.com/](http://ping.chinaz.com/)，进行 `dl.google.com` ping检查，选择大陆响应时间最短的IP地址

![](https://ws3.sinaimg.cn/large/006tNc79ly1fzd5bgepmfj31h90pmjx1.jpg)

![](https://ws2.sinaimg.cn/large/006tNc79ly1fzd5bk15xkj315v0pd45y.jpg)

#### 3. ping 一下这个IP地址

```shell
ping 203.208.40.64
```

> 如果ping得通的话

#### 4. 就把这个地址加入到 hosts 文件中

```
203.208.40.64 dl.google.com
```

> 关于hosts文件位置
>
> Window: C:\WINDOWS\System32\drivers\etc\hosts 
>
> Mac: \etc\hosts

再次打开`Prefernces`

![](https://ws1.sinaimg.cn/large/006tNc79ly1fzd8rmpy3lj31850u0qfn.jpg)

勾选下载



修改自[https://blog.csdn.net/qq_23599965/article/details/80910202](https://blog.csdn.net/qq_23599965/article/details/80910202)

> Android SDK 条目详解（2018.07.04）

![](https://ws3.sinaimg.cn/large/006tNc79ly1fzd8v7mvj3j30u026m1kx.jpg)