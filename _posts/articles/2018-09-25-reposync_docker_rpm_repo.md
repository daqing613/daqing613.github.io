---
layout: post
title: "使用reposync搭建docker本地Centos7 rpm repo"
excerpt: "为了解决部分服务器无法访问docker repo地址" 
categories: articles
tags: ['reposync', 'docker', 'centos7', 'repo']
comments: true
share: true
image:
  feature: so-simple-sample-image-3.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

# 服务端设置

## 安装基本包： 
```bash
yum install yum-utils-1.1.31-42.el7.noarch
```

## 需要在Localrepo 端设置Docker repo。 
Local Repo Server Docker rpm repo setup: 

```bash
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg
```

## 执行如下命令
```
yum makecache fast
```

## 同步已添加的docker-ce repo。 
```bash
reposync  -l --repoid=docker-ce-stable --downloadcomps --download-metadata --download_path=/home/repo/docker/centos7
```

## 生成repodata
```bash
createrepo /home/repo/docker/centos7/docker-ce-stable/
```

# 本地客户端设置

docker.repo
```bash
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=http://mirrors.abc.com/repo/docker/centos7/docker-ce-stable/
enabled=1
gpgcheck=0
gpgkey=https://download.docker.com/linux/centos/gpg
```

执行如下命令
```bash
yum makecache fast 
```
