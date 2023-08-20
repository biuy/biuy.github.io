# 以bridge模式运行Docker
## 1.运行甜糖Docker

```shell
#甜糖
docker pull tiptime/ttnode:latest
#启用bridge模式
docker run -d --name=tiantang --restart=always --privileged --net=bridge --tmpfs /run --tmpfs /tmp -v /EC/docker_space/tiptime/mmcblk0p1:/storage:rw tiptime/ttnode:latest
```

bridge模式下，绑定后能在app中看到网络类型大于0，节点类型低。
遇到无法绑定app端，查看docker ip之后如果不能打开1024(特定端口)，关闭旧的容器重新run:docker stop name ,docker rm container-name
绑定方法：docker logs -f tiantang终端出现二维码，打开手机上的甜糖app扫码绑定设备。

## 2.运行网心云Docker

```shell
#网心云
docker pull onething1/wxedge
#启用bridge模式
docker run -d --name=wxedge --restart=always -p 18888:18888 --privileged --net=bridge  --tmpfs /run --tmpfs /tmp -v /EC/docker_space/wxedge/containers:/storage:rw  onething1/wxedge
```

bridge模式下，绑定后能在app中看到网络类型 映射公网型。
绑定方法：浏览器访问http://主机ip:18888，网页出现二维码。
网心云比甜糖会慢很多，可能需要几个小时才会看到网络类型确定。

# miniupnpd配置方法（网心云和甜糖相同）
最新版本miniupnpd在github上，apt-get的2.0版本与官网2.1版本都存在问题，尝试运行失败了。
https://github.com/miniupnp/miniupnp/tree/master/miniupnpd

## 从github获取最新版
重置代理
```shell
git config --global  --unset https.https://github.com.proxy 
git config --global  --unset http.https://github.com.proxy
```
提醒：检查防火墙 ufw status  ,ufw enable 

`git clone https://github.com/miniupnp/miniupnp.git`后进入miniupnpd文件夹，

或者将miniupnpd文件夹压缩scp上传到服务器。

## 配置为路由

```shell
#开启转发
vi /etc/sysctl.conf
#修改或添加以下内容
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```

## 编译最新的miniupnpd

```shell
cd miniupnpd

#第一次编译可能会缺各种依赖，可以根据报错再安装，以下是常见缺少的包
apt-get install iptables-dev libiptc-dev libssl-dev uuid-dev xz-utils
```

1. 问题：`E: Unable to locate package libiptc-dev`

解决方法：
```shell
# apt-get install pkg-config
apt-get install iptables-dev libiptc-dev
apt-get install pkg-config uuid-dev
apt-get install uuid-dev
```

2. 问题：`error: ‘LOG_WARN’ undeclared (first use in this function); did you mean ‘LOG_KERN’`
解决方法：
```shell
apt-get install xz-utils
```

接着运行
```shell
##注意端口是否被防火墙放行
#设置initial rules and chains.
./netfilter/iptables_init.sh 
./configure
make
make install
```

> 如果没有报错信息就可以顺利安装。如果报错可以安装缺少的包，并make clean后重新安装。

## 配置upnpd
```shell
# 清空iptable
cd /etc/miniupnpd
./miniupnpd_functions.sh
./ip6tables_init.sh 
./iptables_init.sh

# 在文件中查找修改详细配置
vi miniupnpd.conf
# 以下是文件中内容
ext_ifname=eth0 # eth0指WAN接口
ext_ip=(公网地址)
listening_ip=docker0 # docker0指LAN接口
enable_natpmp=yes
enable_upnp=yes
allow 1024-65535 172.17.0.0/24 1024-65535 # 这里的172.17.0.0是docker0网段，不一定是172.17.0.0，可以ifconfig查看一下

# 如果是拨号VPS
ext_ifname=ppp0
#ext_ip=ppp0
listening_ip=docker0
enable_natpmp=yes
enable_upnp=yes
allow 1024-65535 172.17.0.1/16 1024-65535
```

## 调试
```shell
/etc/init.d/miniupnpd start
systemctl status miniupnpd # 判断运行状态

iptables -t nat -vnL # 在Chain MINIUPNPD应该出现很多DNAT项，可能需要等很久。
```

出现新的DNAT意味着upnp协议在正常进行。可以tcpdump抓包查看。例如 `nohup tcpdump -i any -w test.pcap &`
最后能在app中看到网络类型0，节点类型高。

