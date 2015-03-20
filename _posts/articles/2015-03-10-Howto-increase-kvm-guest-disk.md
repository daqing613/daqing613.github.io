---
layout: post
title: "Howto Increase KVM Guest's Disk Space"
excerpt: "An article to test how to increase kvm guest's disk space on Centos7" 
categories: articles
tags: [KVM, virtualization, Centos7]
comments: true
share: true
image:
  feature: so-simple-sample-image-7.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

I have a CentOS7 guest on a CentOS7 KVM host. As i want to setup a repository locally for Dell OpenManage Linux Repository, I have no space to rsync to the whole files. 

I need to extend the CentOS7 guest's disk space. Partitions are LVM based. 

Here are steps that I followed to resize a KVM guest that used LVM internally. 

* Shutdown the VM
* Add more space to the guest's "image file"
* Start the VM 
* Run fdisk inside VM and delete & re-create LVM partition

Processes:

{% highlight bash %}
fdisk /dev/vdb
{% endhighlight bash %}

Create a new primary partition, set type as linux lvm 
{% highlight yaml %}
n
p
1
t
8e
w
{% endhighlight %}

Create a new physical volume and extend the volume group to new volume group
{% highlight yaml %}
pvcreate /dev/vdb1
vgextend /dev/centos /dev/vdb1
{% endhighlight %}

Check the physical volume for free space, extend the logical volume with the free space
{% highlight yaml %}
vgdisplay -v 
lvextend -l+288 /dev/centos/root
{% endhighlight %}

Finally perform an online resize to resize the logical volume, then check the available space
{% highlight bash %}
xfs_growfs /dev/centos/root
df -h 
{% endhighlight bash %}

Apparently Centos 7 uses XFS as the default file system and as a result resize2fs will fail.
