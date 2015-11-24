---
layout: post
title: "Pip Timeout"
excerpt: "当我们使用pip命令时会经常timeout， 所以需要使用国内的镜像源" 
categories: articles
tags: [timeout, pip]
comments: true
share: true
image:
  feature: so-simple-sample-image-5.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---



为了更方便我们使用pip， 需要制定国内镜像源。 


解决办法： 

1， 

{% highlight bash %}

 pip --default-timeout=100 installl pip 

{% endhighlight %}


2, 

pipy国内镜像目前有：
 

http://pypi.douban.com/  豆瓣

http://pypi.hustunique.com/  华中理工大学

http://pypi.sdutlinux.org/  山东理工大学

http://pypi.mirrors.ustc.edu.cn/  中国科学技术大学

{% highlight bash %}

sudo pip  install IPy  -i http://pypi.douban.com/simple  --trusted-host pypi.douban.com 

{% endhighlight bash %}

可以设置默认配置：

编辑文件

sudo vim /root/.pip/pip.conf


{% highlight bash %}

[global]
index-url = http://pypi.douban.com/simple

trusted-host = pypi.douban.com

{% endhighlight bash %}





