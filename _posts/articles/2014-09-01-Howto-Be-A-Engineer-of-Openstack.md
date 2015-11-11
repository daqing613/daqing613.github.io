---
layout: post
title: "如何成为OpenStack工程师 (转)"
excerpt: "很全的资料，虽然时效可能没那么准确，但是也很不错了。" 
categories: articles
tags: [Openstack, python]
comments: true
share: true
image:
  feature: so-simple-sample-image-3.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

Contents [hide] 
1 0 阅读指南 
2 1 OpenStack Hacker 
3 2 基础技能 
3.1 Python 
3.2 Linux 
3.3 Git 
3.4 Unittest 
4 3 OpenStack 基础 
4.1 The 5-minute Overview 
4.2 OpenStack 基本概念 
4.3 简单安装 OpenStack 
4.3.1 环境设置 
4.3.2 devstack 安装 
4.3.3 packstack(RHEL,CentOS) 安装 
4.3.4 deb包安装 
4.3.5 源码安装 
4.4 调戏 OpenStack 
4.5 Python基本库 
4.5.1 WSGI 
4.5.2 重要的库 
4.5.3 TESTING 
4.6 OpenStack基础组件 
4.6.1 RPC组件 
4.6.2 WSGI 
4.7 OpenStack 代码规范 
4.8 Python 深入学习 
5 4 OpenStack 整体架构 
5.1 架构图 
5.2 工作流 
5.2.1 Keystone Workflow 
5.2.2 Nova Workflow 
5.3 OpenStack 核心项目 
6 5 OpenStack 部署/管理 
6.1 OpenStack 自动化部署 
6.2 OpenStack 监控 
7 6 参与 OpenStack 社区 
8 7 OpenStack 二次开发 
9 8 OpenStack 生态圈 
0 阅读指南 

希望本文能够解开你心中萦绕已久的心结，假如是死结，请移步到 https://wiki.openstack.org/wiki/Main_Page
学习OpenStack其实就是学习各种Python库的过程。 
把OpenStack的设计原则贴在你的墙上。 https://wiki.openstack.org/wiki/BasicDesignTenets
1 OpenStack Hacker 

态度：开放、主动、沟通 
影响力：能说、能写、能分享 
四化：自动化、流程化、系统化、文档化 

2 基础技能 

Python 

书籍: 
《python参考手册》 
《python基础教程》 
教程: Codecademy 
挑战: Python Challenge 
文档: Python v2.7.3 documentation 
高阶: 
The Hitchhiker’s Guide to Python! 
Python Module of the Week 
Linux 

书籍: 
鸟哥的Linux私房菜 
《Unix环境高级编程》 
《UNIX系统编程》 
Git 

书籍： 
Pro Git 
GotGitHub 
教程： 
tryGit 
GitImmersion 
进阶： 
visual-git-guide 
a-successful-git-branching-model 
最常用的git命令： Everyday GIT With 20 Commands Or So 
Unittest 

教程: python unittest 
3 OpenStack 基础 

The 5-minute Overview 

OpenStack is a global collaboration of developers and cloud computing technologists producing the ubiquitous open source cloud computing platform for public and private clouds. The project aims to deliver solutions for all types of clouds by being simple to implement, massively scalable, and feature rich. The technology consists of a series of interrelated projects delivering various components for a cloud infrastructure solution. OpenStackcontrols large pools of compute, storage, and networking resources throughout a datacenter, all managed through a dashboard that gives administrators control while empowering their users to provision resources through a web interface. 
openstack-platform 

OpenStack 基本概念 

介绍： https://www.openstack.org/software/
Compute管理员手册(必看)：http://docs.openstack.org/trunk/openstack-compute/admin/content/ch_getting-started-with-openstack.html
OpenStack End User Guide(必看): http://docs.openstack.org/user-guide/content/
Network管理员手册：http://docs.openstack.org/folsom/openstack-network/admin/content/
Object Storage管理员手册：http://docs.openstack.org/folsom/openstack-object-storage/admin/content/
OpenStack文档：http://docs.openstack.org/
OpenStack词汇表：http://docs.openstack.org/glossary/content/glossary.html
使用命令行管理openstack: http://docs.openstack.org/cli/quick-start/content/index.html
OpenStack Wiki: https://wiki.openstack.org/wiki/Main_Page
简单安装 OpenStack 

环境设置 

为了快速安装OpenStack,你要设置最快的apt源(或者设置yum源)和pypi源。 
设置apt源：http://blog.ubuntusoft.com/ubuntu-update-source.html
设置pypi源：http://www.v2ex.com/t/75316
你也可以搭建自己的apt源和pypi源： 

搭建apt源： 
http://blog.ef.net/2012/10/26/unbutu-release-upgrade-with-local-apt-mirror.html
http://www.cnblogs.com/kulin/archive/2012/08/08/2628400.html
搭建pypi源： 
https://pypi.python.org/pypi/bandersnatch
devstack 安装 

使用devstack安装 http://devstack.org
阅读devstack.sh脚本 http://devstack.org
screen的使用：http://www.9usb.net/201002/linux-screen-mingling.html
devstack使用screen管理OpenStack各个服务，所以你要用screen调试OpenStack。 

packstack(RHEL,CentOS) 安装 

git仓库: https://github.com/stackforge/packstack
quickstart: http://openstack.redhat.com/Quickstart
deb包安装 

使用VirtualBox安装OpenStack 
在Ubuntu 12.04上安装OpenStack Folsom版(FlatDHCP+Multihost) 
Ubuntu12.04.2 OpenStack Grizzly 安装（Bridge） 
源码安装 

官方教程: http://docs.openstack.org/install/
调戏 OpenStack 

pdb： 
http://docs.python.org/2/library/pdb.html
http://blog.csdn.net/hackerain/article/details/8373597
http://www.ibm.com/developerworks/cn/linux/l-cn-pythondebugger/
Python基本库 

WSGI 

eventlet.wsgi: http://eventlet.net/doc/examples.html#wsgi-server
webob: http://webob.org/
pecan: http://pecanpy.org/
wsme: http://pythonhosted.org/WSME/
paste: http://pythonpaste.org/
routes: http://routes.readthedocs.org/en/latest/
重要的库 

SQLAlchemy:http://www.sqlalchemy.org/
libvirt: http://libvirt.org/index.html
logging: http://docs.python.org/2/howto/logging-cookbook.html
greenlet: http://greenlet.readthedocs.org/en/latest/
eventlet: http://eventlet.net/
kombu: http://kombu.readthedocs.org/en/latest/
oslo.config: https://wiki.openstack.org/wiki/Oslo#oslo.config
stevedore: http://stevedore.readthedocs.org/en/latest/
TESTING 

PythonTestingToolsTaxonomy: http://wiki.python.org/moin/PythonTestingToolsTaxonomy (all in one) 
testtools：https://readthedocs.org/projects/testtools/
mox：http://code.google.com/p/pymox/wiki/MoxDocumentation
mock：http://www.voidspace.org.uk/python/mock/
tox：http://tox.readthedocs.org/en/latest/
fixtures：https://pypi.python.org/pypi/fixtures
testscenarios：https://pypi.python.org/pypi/testscenarios/
nose：https://nose.readthedocs.org/en/latest/
testrepository：https://testrepository.readthedocs.org/en/latest/MANUAL.html
OpenStack基础组件 

在OpenStack中，有一个重要的项目叫做Oslo(原名是openstack-common),给OpenStack其他项目提供基础组件。 
https://wiki.openstack.org/wiki/Oslo
RPC组件 

RPC组件 
http://blog.ftofficer.com/2010/03/translation-rabbitmq-python-rabbits-and-warrens/
http://www.rabbitmq.com/tutorials/tutorial-six-python.html
http://docs.openstack.org/developer/nova/devref/rpc.html
WSGI 

http://archimedeanco.com/wsgi-tutorial/
OpenStack 代码规范 

Python PEP8 规范: http://www.python.org/dev/peps/pep-0008/
OpenStack HACKING 规范: https://github.com/openstack-dev/hacking/blob/master/HACKING.rst
Python 深入学习 

理解python中optparse.OptionParser类。 http://docs.python.org/library/optparse.html
理解collections.Mapping类。 http://docs.python.org/library/collections.html
分析浅拷贝，深拷贝 http://blog.csdn.net/winterttr/article/details/2590741http://longmans1985.blog.163.com/blog/static/70605475200991603624942/http://book.51cto.com/art/200806/77233.htm
LoggerAdapter类 http://docs.python.org/howto/logging-cookbook.html#context-info
介绍rabbitmq http://blog.ftofficer.com/2010/03/translation-rabbitmq-python-rabbits-and-warrens/http://kombu.readthedocs.org/en/latest/introduction.html#synopsis
Python Decorators入门 http://blog.csdn.net/beckel/article/details/3585352
Python @classmethod @staticmethod的区别。 
五分钟理解元类（Metaclasses） http://www.cnblogs.com/coderzh/archive/2008/12/07/1349735.html
nova中用到的python知识 http://canx.me/2011/12/%E4%B8%80%E4%BA%9Bpython/ 
python中类的总结 http://ipseek.blog.51cto.com/1041109/802243
with的总结 http://effbot.org/zone/python-with-statement.htm * Pool类 http://nullege.com/codes/search/eventlet.pools.Pool
paste模块 http://pythonpaste.org/
python魔术方法 http://pycoders-weekly-chinese.readthedocs.org/en/latest/issue6/a-guide-to-pythons-magic-methods.html
Routes模块 http://routes.readthedocs.org/en/latest/index.html
yield学习 

http://www.pythonclub.org/python-basic/yield
http://blog.donews.com/limodou/archive/2006/09/04/1028747.aspx
http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/
http://www.jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/
4 OpenStack 整体架构 

架构图 

必看： 
http://ken.pepple.info/openstack/2012/09/25/openstack-folsom-architecture/
http://www.solinea.com/2013/06/15/openstack-grizzly-architecture-revisited/
OpenStack架构图，你可以点击放大。 
openstack-logical-arch-folsom 

工作流 

Keystone Workflow 

必看： 
https://www.ibm.com/developerworks/community/blogs/e93514d3-c4f0-4aa0-8844-497f370090f5/entry/openstack_keystone_workflow_token_scoping?lang=zh
http://docs.openstack.org/trunk/openstack-compute/admin/content/keystone-concepts.html
点击可看大图。 

Keystone-workflow 

Nova Workflow 

必看： 
https://www.ibm.com/developerworks/community/blogs/e93514d3-c4f0-4aa0-8844-497f370090f5/entry/openstack_nova_api?lang=en
http://ilearnstack.com/2013/04/26/request-flow-for-provisioning-instance-in-openstack/comment-page-1/
nova-api处理 REST 请求。 
nova-server-request 

nova创建虚拟机的工作流。 
request-flow1 

OpenStack 核心项目 

对各个项目简要分析：http://www.slideshare.net/randybias/state-of-the-stack-april-2013 核心项目的分析： 
Keystone： 
http://docs.openstack.org/developer/keystone/
http://www.slideshare.net/openstackindia/openstack-keystone-identity-service
Glance： 
http://docs.openstack.org/developer/glance/
Nova： 
http://docs.openstack.org/developer/nova/
https://www.ibm.com/developerworks/community/blogs/e93514d3-c4f0-4aa0-8844-497f370090f5/entry/openstack_nova_scheduler_and_its_algorithm27?lang=en
Cinder： 
http://docs.openstack.org/developer/cinder/
Cinder grizzly deep dive pub 
Neutron： 
http://docs.openstack.org/developer/neutron/
http://www.slideshare.net/openstackindia/openstack-quantum-16710792
http://www.slideshare.net/lewtucker/openstack-quantum-network-service
http://www.slideshare.net/openstackindia/openstack-quantum-18418306
Horizon： 
http://docs.openstack.org/developer/horizon/
Swift： 
http://docs.openstack.org/developer/swift/
http://blog.csdn.net/alex890714/article/details/7314780
Swift架构与实践 
Oslo： 
通用机制的分析： 

quota: http://blog.csdn.net/hackerain/article/details/8223125
policy: http://blog.csdn.net/hackerain/article/details/8241691
5 OpenStack 部署/管理 

OpenStack 自动化部署 

Puppet: 
https://wiki.openstack.org/wiki/Puppet-openstack
https://github.com/stackforge/puppet-openstack
https://puppetlabs.com/solutions/openstack/
Fule: Mirantis出品的部署工具，从裸机到OpenStack组件再到HA全部搞定 

https://fuel.mirantis.com/
OpenStack 监控 

OpenStack 监控: http://www.mirantis.com/blog/openstack-monitoring/
6 参与 OpenStack 社区 

都在这里:https://wiki.openstack.org/wiki/Main_Page
山头： https://review.openstack.org/#/admin/groups
向社区提交Patch：https://wiki.openstack.org/wiki/How_To_Contribute
gerrit的使用：https://wiki.openstack.org/wiki/Gerrit_Workflow
Review别人的Patch：https://review.openstack.org
参与IRC Meeting： 
https://wiki.openstack.org/wiki/Meetings
https://wiki.openstack.org/wiki/Mailing_Lists
https://wiki.openstack.org/wiki/People
参与邮件列表讨论：https://wiki.openstack.org/wiki/Mailing_Lists
跟踪OpenStack项目的发展： 
http://www.openstack.org/blog/
http://planet.openstack.org/
https://wiki.openstack.org/wiki/Special:RecentChanges
http://github.com/openstack
https://github.com/stackforge/ (You will like it) 
https://github.com/openstack-dev/
https://github.com/openstack-infra
学习CI：http://ci.openstack.org/
7 OpenStack 二次开发 

开发Nova的扩展API： 
https://www.ibm.com/developerworks/community/blogs/e93514d3-c4f0-4aa0-8844-497f370090f5/entry/openstack_nova_api?lang=zh
https://wiki.openstack.org/wiki/WritingRequestExtensions
http://stephanfr.com/2013/04/07/creating-an-openstack-keystone-helloworld-extension/
http://www.cnblogs.com/willier/archive/2013/05/22/3092961.html
开发Cinder的driver： 
新的driver必须满足 Minimum Features,参考同类型的driver，依葫芦画瓢。 
8 OpenStack 生态圈 

OpenStack幕后的公司：http://www.chenshake.com/behind-the-openstack-company
State of The Stack：http://www.slideshare.net/randybias/state-of-the-stack-april-2013 (一针见血) 
OpenStack贡献排行榜：http://stackalytics.com/
OpenStack实践分享：http://www.mirantis.com/blog/ (mirantis是目前最成功的OpenStack系统集成商)



http://way4ever.com/?p=349


