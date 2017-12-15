---
layout: post
title: "ubuntu 16.04 resolv.conf 配置"
excerpt: "本文描述如何设置nameserver避免服务器重启之后导致dns解析失败" 
categories: articles
tags: ['resolv.conf', 'dns', 'ubuntu', 'dhcpclient']
comments: true
share: true
image:
  feature: so-simple-sample-image-1.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

由于新换了一个工作环境， 需要配置ubuntu测试环境， 突然发现之前设置的nameserver会被重写。 

google了一翻， 发现有两种解决方案。 

# Method1

直接在`/etc/network/interfaces`中指定dns-nameserver 

*dns-nameservers 202.106.0.20 114.114.114.114*

# Method2 

通过修改resolvconf的base文件来生成resolv.conf文件。 

编辑文件`/etc/resolvconf/resolv.conf.d/base`

*nameserver 202.106.0.20*

执行如下命令
```bash
sudo resolvconf -u
```

我们需要把NetworkManager配置文件中dns=dnsmasq注释掉。 
```bash
sudo vim /etc/NetworkManager/NetworkManager.conf
```

```
[main]
plugins=ifupdown,keyfile,ofono
#dns=dnsmasq
```

```bash
sudo service network-manager restart
```

 * [官网](https://apiblueprint.org/)
