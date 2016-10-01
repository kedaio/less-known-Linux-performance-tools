# 

# Sysdig

### **sysdig是什么？**

它是由Draios Inc. 公司于2013年发布的一个全新的、开源Linux操作系统事件实时监测工具。

### sysdig能做什么？

它可以让你实时监测到Linux系统上发生的所有事件，无论这些事件是关于网络，进程，CPU，磁盘I\/O，内存分配还是容器。

用Sysdig官方的话说，你可以认为Sysdig是一个strace、tcpdump、htop、ftop、lsof以及其他传统拍错工具的集合体，并且加入了容器支持，集成了数据（监测到的事件）处理框架以及其它很酷的功能。简言之，它就是一把Linux系统监控和拍错的瑞士军刀。

### Sysdig是如何工作的？

sysdig的监控能力是通过安装一个内核模块来实现的。这个模块在内核模式下拦截系统调用和其他系统事件并进行记录。

### **Sysdig对系统本身的影响？**

官方并无具体的数据来说明sysdig运行时对系统的影响究竟有多大，只是说设计时就是让它在生产环境中运行，是快速而稳定的。从邮件列表中的用户反馈来看，我们可以认为它是安全的。

### **sysdig的安装**

各个Linux发行版都有sysdig软件包，Debian\/Ubuntu可以直接使用apt-get来安装，CentOS\/Redhat\/Fedora可以使用yum\/dnf来安装（假定EPEL源已经安装）。但这种方式安装的sysdig是发行版自己维护的版本，可能并非最新的版本。因此我也推荐根据官方wiki的方式，即使用官方源来安装。

同时也推荐使用官方的安装脚本，它会自动检测你的操作系统及其版本以及其它sysdig依赖的软件包。如果安装失败，它也会告诉你原。在命令行下运行下面的命令开始安装

```
curl -s https://s3.amazonaws.com/download.draios.com/stable/install-sysdig | sudo bash
```

如果安装失败，请参考下方官方wiki中的手动安装方式：

[http:\/\/www.sysdig.org\/wiki\/how-to-install-sysdig-for-linux\/](http://www.sysdig.org/wiki/how-to-install-sysdig-for-linux/)

如果依然有问题，可以去官方邮件列表（[https:\/\/groups.google.com\/forum\/\#!forum\/sysdig](https://groups.google.com/forum/#!forum/sysdig)）看看，或者官方github（[https:\/\/github.com\/draios\/sysdig](https://github.com/draios/sysdig)）的问题列表中看一下是否有人遇到类似的问题。

或许你也可以尝试以容器的方式来运行。官方的安装wiki中有介绍如何使用docker来运行sysdig。

