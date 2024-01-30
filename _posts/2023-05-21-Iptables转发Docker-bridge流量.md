---
layout: post
title:  “Iptables 转发 Docker bridge 流量”
date:   2023-05-21
excerpt: “记录一下怎么用iptables转发bridge模式下的Docker流量”
tag:
- docker
- 笔记
---

> 以下仅讨论在宿主机内。

## Docker flow流向

### Docker作为client端，docker0流入，eth0流出。

在网上看到其他人给出的流向参考：

Docker内：OUTPUT -> POSTROUTING -> 宿主机内：PREROUTING -> FORWARD -> POSTROUTING

这个比较简单，直接把docker0流入PREROUTING的流量转发走就可以了。

```bash
iptables -t nat -I PREROUTING -p udp -i docker0 --dport [target_port] -j REDIRECT --to-port [redirect_port]
```


### Docker作为Server端，eth0流入，docker0流出

流向参考：

宿主机内：PREROUTING -> FORWARD -> POSTROUTING -> Docker内：PREROUTING -> INPUT 

这个iptables rule想了好久，不能直接把eth0流入的PREROUTING的流量转发，也不能把docker0流入的PREROUTING流量转发，因为在宿主机上没有这个流量。甚至还尝试了能不能把POSTROUTING转发走。最后成功写出了正确的：

```bash
iptables -t nat -I PREROUTING -p udp --sport [target_port] -j REDIRECT --to-port [redirect_port]
```