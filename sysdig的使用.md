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



**但是最下面的菜单选项则显示出csysdig和传统的top\/htop或strace等命令是不同的，它有更多的选项，可以对系统行为做更详细的监控，同时还可以对系统上发生的事件进行过滤，只显示你感兴趣的内容。**我们逐一来看一下这些功能选项的作用。



**帮助键： F1**

鼠标点击选项界面上的Help，会按下h或？键，则显示csysdig的简要使用说明（如果是在GUI的终端中，直接按F1键会启动终端自己的帮助界面，这时需用鼠标）。其中会提示csysdig界面下的常用快捷键，刚开始学习csysdig时比较有帮助。

**视图键： F2**

视图就是一个事件过滤器，它让你只看感兴趣的内容，比如I\/O，网络连接，CPU使用率等。csysdig启动后默认显示的就是一个名为processes的视图。后面我们再详细说一下视图。

**过滤键：F4**

F4键可以让你进一步对csysdig显示的内容进行过滤。按下F4键后，在界面的右下角输入字符串，则csysdig会只显示含有指定字符串的内容。除了字符串外，你可以可以按照sysdig的语法来过滤，如“fd.l4proto=tcp”，后面我们介绍sysdig时会介绍sysdig的过滤规则。

**Echo键：F5**

选定某一项后，按下F5键，csysdig会显示针对该进程的文件描述符的操作及内容。

**Dig键： F6**

显示选定项目的sysdig输出。

**视图帮助键： F7**

选定某一个视图后，按下F7键可以显示该视图相关的帮助信息，如每一栏的含义。

**视图动作键： F8**

按下F8键，可以让你对视图中的选定项目采取特定的动作，比如kill，生成core dump，或打印stack等等。

**排序键： F9**

按下F9键，可以让显示按照指定的列来排序。如进程视图下，可以选择以PID或CPU占用率等来排序。可以通过最左面的控制栏用上下键来选择按照哪一列来排序。如下图所示：

![](/assets/Screenshot from 2016-10-08 09-37-39.png)



### **视图**

Csysdig最方便的地方是它的视图（Views）功能。视图可以让你选择查看系统在最近一个取样间隔（默认2秒）中的某些特定类型的事件，比如系统中（或某个进程）的活动连接，发生了那些系统调用，或者系统中所有（或某个）容器的资源使用情况等等。可以在csysdig启动后按下F2键来进入视图选择界面。然后通过上下键来选择视图。选中了某个视图后，右边界面会有对该视图功能的简短说明以及该视图中每一列显示的释义。然后按下回车键即可查看该视图。视图选择的截屏如下：

![](/assets/Screenshot from 2016-10-08 11-55-23.png)

再次按下F2键可以重新进入视图选择界面。

在目前的版本（0.8.0）中，内置的视图有如下几个：

**Connections （连接）**：上次取样期内系统上活动的网络连接。此视图不仅可用于查看系统，还可以查看单个进程，容器或线程的网络连接情况。

**containers（容器）**：列出本机上运行的所有容器及其资源使用情况。选中容器后按回车键可以查看此容器内各个进程的资源使用情况。

**Containers Errors（容器错误）：**显示所有容器的系统错误计数 。错误分为四个类型：文件I\/O，网络I\/O，内存分配和其它

**Directories（目录）：**被访问目录列表。

**Errors（错误）：**系统调用错误。默认按错误发生次数排序。

**File Opens List（打开文件列表）：**列出每个文件打开操作。包括文件路径，相关进程以及结果。

**Files（文件）：**被访问的文件列表。包括I\/O的大小，频率以及错误计数。

**I\/O by Type（I\/O类型）：**按类型分的I\/O量总览。类型分为：文件，目录，网络（IPv4和IPv6），管道，UNIX套接字，信号文件描述符，事件文件描述符，inotify文件描述符。这个视图中有一个TIME项，可以显示进程完成某一I\/O操作所需要的时间（包括等待），这对于排查I\/O相关的性能问题很有帮助。

**K8s Controllers（Kubernetes 控制器）：**所有的Kubernetes控制器及其资源使用情况。

**K8s Namespaces（kubernetes命名空间）：**所有的Kubernetes命名空间及其资源使用情况。

**K8s Pods（kubernetes Pods）：**所有的Kubernetes Pods及其资源使用情况。

**K8s Services（Kubernetes 服务）：**所有的Kubernetes服务及其资源使用情况。

**New Connections \(新连接）：**所有的新建网络连接

**Page Faults（内存缺页）：**进程启动以来所有的内存缺页计数。包括Major和Minor。

**Processes（进程）：系统上的进程列表。类似于top\/htop。**

**Processes CPU（进程CPU）：**进程的CPU使用情况。

**Processes Errors（进程错误）：**进程的系统错误计数。并按类型分为文件、网络、内存和其它。

**Processes FD Usage（进程文件描述符使用）：**进程使用（打开）了多少文件文件描述符，以及它最多能使用的数量。

**Server Ports（服务器端口）：列**出所有的服务器端口以及它们的带宽使用情况。

**Sockets Queue（套接字队列）：**每个进程的套接字队列（backlog）使用情况。所谓backlog，就是每个进程最多允许多少等待建立的连接。当这个队列或backlog达到最大值后，新的连接请求将被拒绝。

**Spectrograme-file（文件柱状图）：**文件I\/O延迟的柱状图。横轴为延迟的时间，纵轴为计数。

**Spy Syslog（监视系统日志）：**显示写入到系统日志的内容。

**Spy User（监视用户）：显示所有以互动方式运行的命令。**

**System Calls（系统调用）：**系统调用的触发频率以及每次调用完成所花费的时间。

**Threads（线程）：**系统上所有线程列表，以及它所属的进程和资源使用情况。

https:\/\/sysdig.com\/blog\/linux-troubleshooting-cheatsheet\/

https:\/\/sysdig.com\/blog\/fascinating-world-linux-system-calls\/

https:\/\/sysdig.com\/blog\/interpreting-sysdig-output\/

https:\/\/sysdig.com\/blog\/sysdigkubernetes-adventure-part-1-kubernetes-services-work\/

https:\/\/sysdig.com\/blog\/a-sysdigkubernetes-adventure-part-2-troubleshooting-kubernetes-services\/

