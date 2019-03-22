# linux 

### 1、ps

`ps -ef`

ps -ef 是用标准的格式显示进程的；

`ps -aux `

ps aux 是用BSD的格式来显示,可以查看进程的STAT状态；

###2、netstat

`netstat -pantu`

-l:显示侦听的端口；-a:列出所有端口；-p:显示进程名称；-n:直接使用ip地址，而不通过域名服务器;-t和-u：显示TCP和UDP端口

### 3、vmstat

对操作系统的虚拟内存、进程、IO读写、CPU活动等进行监视

r:等待运行的进程数。如果等待运行的进程数越多，意味着CPU非常繁忙。

b:处在非中断睡眠状态的进程数。意味着进程被阻塞。

swpd:已使用的虚拟内存大小。swapd不为0，并不意味物理内存吃紧，如果swapd没变化，si、so的值长期为0,这也是没有问题的;

free:空闲的物理内存的大小

si:如果这个值大于**0**，表示物理内存不够用或者内存泄露了;

so:如果这个值大于**0**，表示物理内存不够用或者内存泄露了;

**如果这2个值长期大于0时，系统性能会受到影响，磁盘IO和CPU资源都会被消耗。**

 us:用户CPU时间(非内核进程占用时间)（单位为百分比）

sy:sy的值高时，说明系统内核消耗的CPU资源多;

id:系统使用的CPU时间（单位为百分比）;

wa:wait越大则机器io性能就越差。

### 4、top

top - 01:54:38 up 30 days, 12:06,  2 users,  load average: 0.21, 0.21, 0.20

系统当前时间 up  系统到目前为止i运行的时间， 当前登陆系统的用户数量，  load average后面的三个数字分别表示距离现在一分钟，五分钟，十五分钟的负载情况。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了;

Tasks: 168 total,   2 running, 166 sleeping,   0 stopped,   0 zombie

tasks表示任务（进程），240 total则表示现在有240个进程，其中处于运行中的有2个，238个在[休眠](https://www.baidu.com/s?wd=%E4%BC%91%E7%9C%A0&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)（挂起），stopped状态即停止的进程数为0，zombie状态即僵尸的进程数为0个。

%Cpu(s):  7.1 us,  3.6 sy,  0.0 ni, 89.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

User用户空间占用cpu的百分比 ;system 内核空间占用cpu的百分比 ;

MiB Mem :   1996.9 total,     95.9 free,   1792.0 used,    109.1 buff/cache

物理内存总量（3.7G),空闲内存总量（2.5G),使用中的内存总量（2.4G),缓冲内存量 

MiB Swap:   2045.0 total,    296.2 free,   1748.8 used.     76.3 avail Mem

交换区总量（4G），空闲交换区总量（2.7G),使用的交换区总量（1.2G），可用交换取总量;

### 5、crontab

1、口诀“分、时、日、月、星期 命令”

[root@linuxprobe ~]# crontab -e

[root@linuxprobe ~]# crontab -l

25 3 * * 1,3,5 /usr/bin/tar -czvf backup.tar.gz /home/wwwroot

0 1 * * 1-5 /usr/bin/rm -rf /tmp/*

在crond服务的计划任务参数中，所有命令一定要用绝对路径的方式来写；

whereis查看命令的绝对路径

###6、tree

查看文件目录下所有的文件夹和文件，并以树形结构展示；

