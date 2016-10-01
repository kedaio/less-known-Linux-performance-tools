

使用sysdig需要root权限，因为它需要读\/proc虚拟文件系统和相关的虚拟设备文件，必要时还要重新加载sysdig\_probe内核模块。



使用sysdig有两种方式。一是直接使用sysdig命令，另一个是使用它的基于ncurses的用户界面：csysdig。

csysdig的界面看上去和top\/htop差不多，用起来也更直观一些，我们就先从它开始。在命令行下运行

```
sudo csysdig
```

就会看到如下界面。是不是和top命令很像？

![](/assets/Screenshot from 2016-10-01 22-05-43.png)

它列出了系统中正在运行的进程的信息，包括进程ID（PID）,占用的CPU（CPU，以百分比计），包含的线程数（TH），总虚拟内存（VIRT），实际使用的物理内存（RES），产生的文件I\/O（FILE，以byte\/秒计，包括输入和输出），产生的网络I\/O（NET，以byte\/秒计，包括输入和输出）等等。



