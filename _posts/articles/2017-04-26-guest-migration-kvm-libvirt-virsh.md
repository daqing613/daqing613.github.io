---
layout: post
title: "guest migration related technical skills"
excerpt: "本迁移手册着重介绍非共享存储， 非LVM卷， 采用两种不通的迁移方式来实现迁移, 主要应用场景是KVM虚拟化下的guest迁移。" 
categories: articles
tags: ['kvm', 'libvirt', 'migration', 'virsh']
comments: true
share: true
image:
  feature: so-simple-sample-image-3.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---



# 方法一: 

### 若我们使用的是共享存储， 这里我们里可以直接使用virsh命令行来迁移我们的虚拟机。 

#### 在线迁移

`virsh migrate --live guest1-rhel6-64 qemu+ssh://host2.example.com/system`

#### 关机迁移， 同时删除源宿主机上的虚拟机

`virsh migrate --persistent --undefinesource guest_VM qemu+ssh://secondHost.example.com/system`

### 假如我们使用的是本地磁盘存储的虚拟机磁盘文件， 我们需要添加--copy-storage-all来迁移磁盘文件。 

`virsh migrate --persistent --undefinesource --copy-storage-all guest_VM qemu+ssh://secondHost.example.com/system`

# 方法二： 

这里我们可以使用dumpxml将虚拟机的信息写入到一个xml文件中。然后在目的节点依据该xml文件来启动虚拟机。 

### 基于block存储的VM迁移： 

  1. copy the VM's disks from /var/lib/libvirt/images on src host to the same dir on destination host
  2. on the source host run `virsh dumpxml VMNAME > domxml.xml` and copy this xml to the dest. host
  3. on the destination host run `virsh define domxml.xml` start thew VM.

   If the disk location differs, you need to edit the xml's devices/disk node to point to the image on the destination host. 

### 自定义网络迁移： 

   If the VM is attached to custom defined networks, you'll need to either edit them out of the xml on the destination host or 
   redefine them as well: 
   ```
   virsh net-dumpxml > netxml.xml  
   virsh net-define netxml.xml
   virsh net-start NETNAME
   virsh net-autostart NETNAME
   ```

### 迁移快照：
   If the VM has snapshots that you want to preserve, you should dump the snapshot xml-files on the source:  
   ```
   virsh snapshot-list --name $dom 
   virsh snapshot-dumpxml $dom $name > file.xml 
   virsh snapshot-create --redefine $dom file.xml
   ```
   to finish migrating the snapshots.
   If you also care about which snapshot is the current one, then additionally do on the source:
   `virsh snapshot-current --name $dom`
   and on the destination:
   `virsh snapshot-current $dom $name`
   Then you can use `virsh snapshot-delete --metadata $dom $name` for each snapshot to delete the xml files on the source, or you could just delete them from /var/lib/libvirt/qemu/snapshots/\$guestname


# Note:
   当我们执行live-migration时，对于某些内存密集型的虚拟机是无法迁移成功的， 由于它持续写入内存; 这里我们可以通过执行如下命令： 
   `virsh migrate-setmaxdowntime $dom $time`
   来设置一个最大可接受的停机时间， 这个制定的时间是毫秒来计算的。 

> *引用地址*

>    https://serverfault.com/questions/434064/correct-way-to-move-kvm-vm
>    https://libvirt.org/migration.html  
>    https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Virtualization_Deployment_and_Administration_Guide/sect-KVM_live_migration-Live_KVM_migration_with_virsh.html
