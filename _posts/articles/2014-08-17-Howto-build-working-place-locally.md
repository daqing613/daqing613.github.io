---
layout: post
title: "How to Set up A Local Working Place for blog test."
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

每次更换公司之后都需要重新搭建一套测试环境， 其中就有关于本地blog的测试环境(jekyll)， 一般会包含两步: 软件环境的安装， blog的同步。 


## 软件环境准备

依赖包安装: 
```bash
    sudo apt-get install build-essential
    sudo apt-get install ruby-full
    sudo gem update --system
```
安装Jekyll相关包: 
```bash
    # Install Jekyll and Bundler gems through RubyGems
    gem install jekyll bundler jekyll-sitemap
```

## 克隆blog


* 上传public key 

首先我们需要把本地的publickey 上传到github上， 内容很简单，不细描述。 


* git clone

```bash
git clone git@github.com:daqing613/daqing613.github.io.git 

cd daqing613.github.io/ 

git remote set-url origin git@github.com:daqing613/daqing613.github.io.git

git push origin master 

git config --global user.email "dwong@dwong.com"

git config --global user.name "Dwong"
```

>引用地址

* https://jekyllrb.com/docs/installation/#requirements 
* https://www.ruby-lang.org/en/documentation/installation/#apt
* https://rubygems.org/pages/download
* https://jekyllrb.com/docs/quickstart/

