---
layout: post
title: "How to Set up A Local Working Place for www.dwong.in"
excerpt: "有些时候我们会需要更换电脑，但是我们仍然需要对自己的blog进行update,这是就需要创建本地环境。" 
categories: articles
tags: [git, jekyll, linux, blog]
comments: true
share: true
image:
  feature: so-simple-sample-image-1.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

由于需要在公司同时对自己的blog进行更新， 我要在公司搭建一个环境， 下面就是如何来克隆github上的内容到本地，及建立ssh认证通道。 



* 上传public key 

首先我们需要把本地的publickey 上传到github上， 内容很简单，不细描述。 


* git clone

{%  highlight bash %}

git clone git@github.com:daqing613/daqing613.github.io.git 

cd daqing613.github.io/ 

git remote set-url origin git@github.com:daqing613/daqing613.github.io.git

git push origin master 

git config --global user.email "dwong@dwong.com"

git config --global user.name "Dwong"

{% endhighlight %}





