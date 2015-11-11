---
layout: post
title: "How to install munin on centos6"
excerpt: "It's easy to see the trends for monitoring resource" 
categories: articles
tags: [munin, lamp]
comments: true
share: true
image:
  feature: so-simple-sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
link: http://www.unixmen.com/install-munin-monitoring-tool-centos-rhel-scientific-linux-6-56-46-3/
---



Munin is networked resource monitoring tool, with which we can see the trends directly. 


First, we need the lamp installed. If you have no idea about it. Google it. 


Second, 

{% highlight bash %}

yum install munin munin-node -y
vi /etc/munin/munin.conf
[server.dwong.local]
address 127.0.0.1

{% endhighlight %}


It's easy to install and use. 

Also, we can integrate the datas. 

we can integrate two net interface sum . 

{% highlight bash %}

[mydomain.com;aggregates]
    total_bandwidth.graph_args --base 1000 -l 0
    total_bandwidth.cdef 0
    total_bandwidth.graph_category Network
    total_bandwidth.graph_title Aggregated bandwidth
    total_bandwidth.graph_vlabel Bits/sec
    total_bandwidth.upload.label upload
    total_bandwidth.total.graph yes
    total_bandwidth.upload.sum \
    mybox1.mydomain.com:if_eth0.up \
    mybox2.mydomain.com:if_eth0.up
    total_bandwidth.upload.type COUNTER
    total_bandwidth.download.type COUNTER
    total_bandwidth.download.label download
    total_bandwidth.graph_order upload download
    total_bandwidth.total.graph no
    total_bandwidth.download.sum \
    mybox1.mydomain.com:if_eth0.down \
    mybox2.mydomain.com:if_eth0.down


{% endhighlight bash %}



Reference:


`http://munin-monitoring.org/`





