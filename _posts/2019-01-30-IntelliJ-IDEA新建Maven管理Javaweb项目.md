---
layout: post
title:  "IntelliJ IDEA新建Maven管理Javaweb项目"
date:   2019-01-30
excerpt: "IntelliJ IDEA 2018的使用"
image: https://ws2.sinaimg.cn/large/006tNc79gy1fzffs2jymaj31hc0u0jys.jpg
tag:
- javaweb
- web
---

# IntelliJ IDEA新建Maven管理Javaweb项目

> 版本：IntelliJ IDEA 2018.3.3 (Ultimate Edition)
> Build #IU-183.5153.38, built on January 9, 2019
> JRE: 1.8.0_152-release-1343-b26 x86_64
> JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
> macOS 10.14.3

遇到各种问题网上不太容易查到对应版本的解决方法

> 持续更新

------

## 新建一个Javaweb项目

### 常规步骤

选择Maven，勾上Create from archetype，选择webapp的原型模板：

![](https://ws1.sinaimg.cn/large/006tNc79gy1fzowlhccx7j30rs0lbwg7.jpg)

下一步，填入GroupId、ArtifactId等：

![](https://ws1.sinaimg.cn/large/006tNc79gy1fzowmuq0urj30h604aglj.jpg)

然后再下一步：

![](https://ws1.sinaimg.cn/large/006tNc79gy1fzown1naqyj30mp09wdg7.jpg)

据说这里在Properties中添加一个参数archetypeCatalog=internal，不加这个参数，在maven生成骨架的时候将会非常慢，有时候会直接卡住。

不过加了这个我也没感觉会变快（

然后`Finish`应该就建好了

### 配置Tomcat

点击图中工具栏中的向下箭头图标，单击Edit Configurations；或者在菜单栏Run→ Edit Configurations:

![](https://ws4.sinaimg.cn/large/006tNc79gy1fzowplyohtj30rs0f8t9u.jpg)

点击➕后选Tomcat Server > Local

![](https://ws3.sinaimg.cn/large/006tNc79gy1fzowpyyq6xj30rs0hwdge.jpg)

选好Tomcat

![](https://ws3.sinaimg.cn/large/006tNc79gy1fzowrqa856j30rs0id3zc.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79gy1fzowtri4tej30rs0idwfc.jpg)

好了以后，点击页签Deployment，然后点击 + 选择 Artifact …：

![](https://ws2.sinaimg.cn/large/006tNc79gy1fzows4zqejj30rs0iejrr.jpg)

然后选

![](https://ws1.sinaimg.cn/large/006tNc79gy1fzowuap8x3j30qg0rkt93.jpg)

就可以运行啦

#### 问题来了。。。并没有出现Artifact…….....

打开`Project structure`，点➕

![](https://ws1.sinaimg.cn/large/006tNc79gy1fzowvmol8aj311k0u040u.jpg)

然后出现应用

![](https://ws3.sinaimg.cn/large/006tNc79gy1fzowx32rrrj30rw0ri3yo.jpg)

点击后OK

![](https://ws4.sinaimg.cn/large/006tNc79gy1fzowxs0y1cj311l0u03zu.jpg)

然后在Tomcat配置的步骤里出现了这个Artifact，运行也是非常顺利的

![image-20190130210704923](/Users/biu/Library/Application Support/typora-user-images/image-20190130210704923.png)

