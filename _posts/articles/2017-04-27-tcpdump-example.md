---
layout: post
title: "tcpdump example"
excerpt: "本文档介绍一些tcpdump常用的命令" 
categories: articles
tags: ['network', 'tcpdump', 'linux']
comments: true
share: true
image:
  feature: so-simple-sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---


这里我们需要使用root权限在网卡上来抓包。 建议使用tcpdump抓包并保存到一个文件里， 然后使用Wireshark来分析它。 

* 列出可以被tcpdump监听的网卡： 

`tcpdump -D`

* 监听eth0网卡:

`tcpdump -i eth0`

* 监听所有可用的网卡(不能用于*混杂模式*. 需要linux kernel 2.2及以上)

`tcpdump -i any`

* 抓包处于详细模式

`tcpdump -v`

* 抓包处于详细模式, 程度+1

`tcpdump -vv`

* 抓包处于详细模式, 程度+2

`tcpdump -vvv`

* 详细模式并同时以十进制和ASCII的方式打印每个包的数据， 排除链路层数据头

`tcpdump -v -X`

* 详细模式并同时以十进制和ASCII的方式打印每个包的数据， 包含链路层数据头

`tcpdump -v -XX`


* 抓包处于低于默认详细模式下

`tcpdump -q`

* 抓100个包： 

`tcpdump -c 100`

* 抓包并写入到capture.cap文件

`tcpdump -w capture.cap`

* 抓包并写入到capture.cap文件同时实时显示包的数量

`tcpdump -v -w capture.cap`

* 从文件capture.cap中读取数据包

`tcpdump -r capture.cap`

* 从文件capture.cap中读取数据包的最详细信息

`tcpdump -vvv -r capture.cap`

* 抓包时显示IP地址和端口而不是域名和服务名(在某些系统上我们可能需要指定*--n*来显示端口号)

`tcpdump -n`

* 抓取所有到目的地址192.168.1.1的包。 显示IP地址和端口号

`tcpdump -n dst host 192.168.1.1`

* 抓取所有源地址为192.168.1.1的包。 显示IP地址和端口号

`tcpdump -n src host 192.168.1.1`

* 抓取所有不论目的地址还是源地址是192.168.1.1的包。 显示IP地址和端口号

`tcpdump -n host 192.168.1.1`

* 抓取所有到目的网络192.168.1.0/24的包。 显示IP地址和端口号

`tcpdump -n dst net 192.168.1.0/24`

* 抓取所有源网络192.168.1.0/24的包。 显示IP地址和端口号

`tcpdump -n src net 192.168.1.0/24`

* 抓取所有不论是目的网络还是源网络192.168.1.0/24的包。 显示IP地址和端口号

`tcpdump -n net 192.168.1.0/24`

* 抓取所有目的端口为23的包。 显示IP地址和端口号

`tcpdump -n dst port 23`

* 抓取所有目的端口为1-1023的包。 显示IP地址和端口号

`tcpdump -n dst portrange 1-1023`

* 抓取所有目的端口为1-1023的TCP包。 显示IP地址和端口号

`tcpdump -n tcp dst portrange 1-1023`

* 抓取所有目的端口为1-1023的UDP包。 显示IP地址和端口号

`tcpdump -n udp dst portrange 1-1023`

* 抓取所有目的IP为192.168.1.1和端口为23的包。 显示IP地址和端口号

`tcpdump -n "dst host 192.168.1.1 and dst port 23"`

* 抓取所有目的IP为192.168.1.1和端口为443或80的包。 显示IP地址和端口号

`tcpdump -n "dst host 192.168.1.1 and (dst port 80 or dst port 443)"`

* 抓取所有ICMP包

`tcpdump -v icmp`

* 抓取所有ICMP包

`tcpdump -v arp`

* 抓取所有ICMP和ARP包

`tcpdump -v "icmp or arp"`

* 抓取广播或者多播的包 

`tcpdump -n "broadcast or multicast"`

* 抓取大小为500字节的包，而不是默认的68字节包

`tcpdump -s 500`

* 抓取所有字节大小的包

`tcpdump -s 0`

> http://www.rationallyparanoid.com/articles/tcpdump.html 
