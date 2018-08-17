---
layout: post
title: Bandwagon 科学上网
tags: [other]
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
  

