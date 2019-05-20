---
layout: post
title: Bandwagon 科学上网
category: 工具
keywords: bandwagon
---
# 购买VPS
[Bandwagon](https://bandwagonhost.com/aff.php?aff=35524)
19.99$一年，优惠码BWH26FXH3HIQ (折扣幅度: 6.25%)
支付宝可用

# Install new OS
选择Centos-7-x86_64，reload
稍等片刻，记下显示的root密码和new ssh port

# Install ShadowSocks
```
yum install wget traceroute net-tools -y

wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```
选择运行1 shadowsocks-python

# Install BBR
```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
```
完成后，提示重启，输入y重启VPS

重启后，验证是否成功安装，
1. 输入：
```
uname -r
```
查看内核版本，含有 4.10 就表示 OK 了

1. 输入
```
sysctl net.ipv4.tcp_available_congestion_control
```
返回值一般为：
net.ipv4.tcp_available_congestion_control = bbr cubic reno

1. 输入
```
sysctl net.ipv4.tcp_congestion_control
```
返回值一般为：
net.ipv4.tcp_congestion_control = bbr

1. 输入
```
sysctl net.core.default_qdisc
```
返回值一般为：
net.core.default_qdisc = fq

1. 输入
```
lsmod | grep bbr
```
返回值有 tcp_bbr 模块即说明bbr已启动。

  
# Install shadowsocks-libev by docker
```
docker run -d -p xx:xx -p xx:xx/udp --name ss-libev -v /etc/shadowsocks-libev:/etc/shadowsocks-libev teddysun/shadowsocks-libev
```

# Install L2TP by docker

1. l2tp
```
docker run -d --privileged -p 500:500/udp -p 4500:4500/udp --name l2tp --env-file /etc/l2tp.env -v /lib/modules:/lib/modules teddysun/l2tp
```

1. 开通防火墙
```
iptables -A INPUT  -p udp --dport 500 -j ACCEPT
iptables -A INPUT  -p udp --dport 4500 -j ACCEPT
iptables -A INPUT  -p 50 -j ACCEPT
iptables -A INPUT  -p 51 -j ACCEPT
iptables -A INPUT  -p udp -m policy --dir in --pol ipsec -m udp --dport 1701 -j ACCEPT
service iptables save
service iptables restart
iptables -L -n
```

协议：UDP,端口500(用于IKE,用于管理加密密钥)
协议：UDP,端口4500(用于IPSEC NAT-Traversal模式)
协议：ESP,价值50(IPSEC)
协议：AH,值51(对于IPSEC)
此外,L2TP服务器使用端口1701,但不允许从外部对其进行连接.有一个特殊的防火墙规则,只允许此端口上的IPSEC安全流量入站.

# 其他dockers命令
1. 所有容器开机自启
```
docker update --restart=always $(docker ps -a -q)
```

1. docker日志查看
```
docker logs l2tp
```

1. docker安装
```
sudo yum install -y yum-utils   device-mapper-persistent-data   lvm2
sudo yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce
sudo systemctl start docker
systemctl enable docker
```

1. docker-compose安装
```
curl -L https://github.com/docker/compose/releases/download/1.24.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
