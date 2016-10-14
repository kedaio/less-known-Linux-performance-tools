使用sysdig需要root权限，因为它需要读\/proc虚拟文件系统和相关的虚拟设备文件，必要时还要重新加载sysdig\_probe内核模块。

使用sysdig有两种方式。一是直接使用sysdig命令，另一个是使用它的基于ncurses的用户界面：csysdig。

csysdig的界面看上去和top\/htop差不多，用起来也更直观一些，我们就先从它开始。在命令行下运行

```
sudo csysdig
```

就会看到如下界面。是不是和top命令很像？

![](/assets/Screenshot from 2016-10-01 22-05-43.png)

它列出了系统中正在运行的进程的信息，包括进程ID（PID）,占用的CPU（CPU，以百分比计），包含的线程数（TH），总虚拟内存（VIRT），实际使用的物理内存（RES），产生的文件I\/O（FILE，以byte\/秒计，包括输入和输出），产生的网络I\/O（NET，以byte\/秒计，包括输入和输出）等等。

默认csysdig每两秒刷新一次。可以使用-d或--delay在csysdig启动时指定刷新间隔（单位以毫秒计）。和top命令的-d参数一样（但时间单位不同）。

如果你对某一个进程感兴趣，可以通过上下键选择该进程后按下回车键，则csysdig会只显示该进程的有关信息，比如它有几个线程，每个线程的CPU占用率，文件I\/O等等。

最下面的这些选项显示了csysdig和其它命令的不同。按照提示，**按下F9键，可以让显示按照PID或CPU占用率等来排序**。可以通过最左面的控制栏用上下键来选择按照哪一列来排序。如下图所示：

![](/assets/Screenshot from 2016-10-08 09-37-39.png)

Csysdig最方便的地方是它的视图（Views）功能。视图可以让你选择查看系统在最近一个取样间隔（默认2秒）中的某些特定类型的事件，比如系统中（或某个进程）的活动连接，发生了那些系统调用，或者系统中所有（或某个）容器的资源使用情况等等。可以在csysdig启动后按下F2键来进入视图选择界面。然后通过上下键来选择视图。选中了某个视图后，右边界面会有对该视图功能的简短说明以及该视图中每一列显示的释义。然后按下回车键即可查看该视图。视图选择的截屏如下：

![](/assets/Screenshot from 2016-10-08 11-55-23.png)

再次按下F2键可以重新进入视图选择界面。

在目前的版本（0.8.0）中，内置的视图有如下几个：

**Connections （连接）**：上次取样期内系统上活动的网络连接。此视图不仅可用于查看系统，还可以查看单个进程，容器或线程的网络连接情况。

**containers（容器）**：列出本机上运行的所有容器及其资源使用情况。选中容器后按回车键可以查看此容器内各个进程的资源使用情况。

**Containers Errors（容器错误）：**显示所有容器的系统错误计数 。错误分为四个类型：文件I\/O，网络I\/O，内存分配和其它

**Directories（目录）：**被访问目录列表。

**Errors（错误）：**系统调用错误。默认 按错误发生次数排序。

**File Opens List（打开文件列表）：**列出每个文件打开操作。包括文件路径，相关进程以及结果。

**Files（文件）：被访问的文件列表。**

**I\/O by Type（I\/O类型）：**

**K8s Controllers（Kubernetes 控制器）：**

**K8s Pods（kubernetes Pods）：**

**K8s Services（Kubernetes 服务）：**

**New Connections \(新连接）：**

**Page Faults（内存页面错误）：**

**Processes（进程）：**

**Processes CPU（进程CPU）：**

**Processes FD Usage（进程文件描述符使用）：**

**Server Ports（服务器端口）：**

**Sockets Queue（套接字队列）：**

**Spectrograme-file（）：**

**Spy Syslog（监视系统日志）：**

**Spy User（监视用户）：**

**System Calls（系统调用）：**

**Threads（线程）：**

https:\/\/sysdig.com\/blog\/linux-troubleshooting-cheatsheet\/

https:\/\/sysdig.com\/blog\/fascinating-world-linux-system-calls\/

https:\/\/sysdig.com\/blog\/interpreting-sysdig-output\/

https:\/\/sysdig.com\/blog\/sysdigkubernetes-adventure-part-1-kubernetes-services-work\/

https:\/\/sysdig.com\/blog\/a-sysdigkubernetes-adventure-part-2-troubleshooting-kubernetes-services\/

