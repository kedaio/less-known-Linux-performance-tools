# 

# Sysdig

sysdig是由Draios Inc. 公司发布的一款Linux事件监测工具。可以让你了解Linux系统上发生的所有事件。而且它支持容器（container），这相对于传统的工具（如top，lsof等等）是一个很大的优势。

用Sysdig官方的话说，Sysdig相当于strace + tcpdump + htop + iftop + lsof +......。或者你可以把它想象成一个系统记录仪，它可以记录下Linux系统的一举一动。剩下的就看你如何解读这些监控记录了。

sysdig的监控能力是通过安装一个内核模块来实现的。这个模块在内核模式下拦截系统调用和其他系统事件并进行记录。

官方并无具体的数据来说明sysdig运行时对系统的影响究竟有多大，只是说

