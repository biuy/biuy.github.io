---
layout: post
title:  "Mac 上 Human Resource Machine 更改中文"
date:   2023-12-29
excerpt: "Epic免费领的程序员升职记改中文方法。"
tag:
- 笔记
---

> 不知道为啥网上竟然找不到Mac上Epic下载的程序员升职记怎么改中文，好不容易找到路径，记录一下

Epic下载的程序员升职记对应的`settings.txt`文件位置

```
$ cd ~/Library/Containers/Tomorrow-Corporation.Human-Resource-Machine/Data/Library/Application\ Support/Human\ Resource\ Machine
```

在这个文件里把language从system改成中文就可以了。

文件内容
```
fullscreen = 0
language = zh
volume_music = 1.0
volume_sfx = 1.0
```

源文件里写了各种语言的资源文件，就是不给我菜单选择，可能升职不了的程序员不能玩中文版。