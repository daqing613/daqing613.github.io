---
layout: post
title: "minicom example"
excerpt: "本文档接受如何在linux系统下链接串行设备。" 
categories: articles
tags: ['minicom', 'ubuntu', 'serial console', 'pl2303 converter']
comments: true
share: true
image:
  feature: so-simple-sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

通常windows上面我们可以通过设备管理中查看串行设备链接到具体的哪个com口，以及各种图形化界面的链接工具;这里我们介绍一下如何在linux上连接串行设备。 

这里我们用的是*minicom*, 所使用的是ubuntu系统。 

安装minicom: 

`sudo apt-get install minicom`

由于电脑上没有串行口， 这里我们需要使用到usb转换器(pl2303)

`dmesg | grep tty`
```
  [    0.000000] console [tty0] enabled
  [49689.082419] usb 3-2: pl2303 converter now attached to ttyUSB0
```

打开minicom, 修改串行口设置

`minicom -s -c on`

```
   +-----------------------------------------------------------------------+   
   | A -    Serial Device      : /dev/ttyUSB0                              |   
   | B - Lockfile Location     : /var/lock                                 |   
   | C -   Callin Program      :                                           |   
   | D -  Callout Program      :                                           |   
   | E -    Bps/Par/Bits       : 9600 8N1                                  |   
   | F - Hardware Flow Control : Yes                                       |   
   | G - Software Flow Control : No                                        |   
   |                                                                       |   
   |    Change which setting?                                              |   
   +-----------------------------------------------------------------------+  

```

cheers!

> *引用地址*  
>https://askubuntu.com/questions/634180/minicom-status-stays-offline


