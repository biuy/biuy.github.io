---
layout: post
title:  “快速配置linux apache服务器断点续传”
date:   2023-12-21
excerpt: "windows电脑上有很大的硬盘，但几十个G的数据总要传到linux上。”
tag:
- linux
- 笔记
---

> 代码和数据总存在linux上。windows没有rsync不能直接断点下载太难受了，每次下载超过10个G都会一直下不下来。


# linux

```shell
sudo apt-get install apache2
```
> 备份文件位置`/backup`

链接 `ln -s /backup /var/www/html/file`

> 取消链接 `unlink backup`

启动/重启/关闭apache

```shell
systemctl start/restart/stop apache2
```

然后访问 `[ip]/download`就可以下载文件。

> 改端口可以修改 `/etc/apach2/ports.conf`

## 身份认证

### 密码

使用htpasswd工具

```shell
htpasswd -c users username
```

> 添加新用户去掉`-c`。

密码文件名 `users`，新建目录`mkdir /etc/apache2/authentication`，然后移动密码文件`mv users /etc/apache2/authentication/`。

### 配置

配置文件 `/etc/apache2/sites-enabled/000-default.conf`

加入
```bash
        #Download Authentication
        <Directory "/var/www/html/download">
                AuthType Basic
                AuthName "Download"
                AuthUserFile /etc/apache2/authentication/users
                Require valid-user
        </Directory>

```

如有报错，log位置 `/var/log/apache2`


# windows

断点续传（带用户名密码）

```shell
curl.exe -C - -O -u user:passwd http://[ip]/download/XX.zip
```