#### 简介

nmon是一种在 AIX 与各种 Linux 操作系统上广泛使用的监控与分析工具，它能在系统运行过程中实时地捕捉系统资源的使用情况，可将服务器系统资源耗用情况收集起来并输出一个特定的文件，并可利用 excel 分析工具（nmon analyser）进行数据的统计分析

#### 安装

##### 系统环境：

```linux
[root@i-sgtj39fd /]# cat /proc/version
Linux version 3.10.0-957.21.3.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC) ) #1 SMP Tue Jun 18 16:35:19 UTC 2019
```

##### 更新yum源：

```linux
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

##### 安装：

```
yum install -y nmon
```

#### 使用

##### nmon快捷命令

```q : 停止并退出 nmon
q : 停止并退出nmon
h : 查看帮助
c : 查看 CPU 统计数据
m : 查看内存统计数据
d : 查看硬盘统计数据
k : 查看内核统计数据
n : 查看网络统计数据
N : 查看 NFS 统计数据
j : 查看文件系统统计数据
t : 查看高耗进程
V : 查看虚拟内存统计数据
v : 详细模式
```

##### 命令行参数

```
nmon -f -t -s 10 -c 180 -m /tmp/
     -f          生成数据文件名中包含文件创建的时间
     -t          输出中包括占用率较高的进程
     -s 10       每10秒采集一次数据
     -c 180      采集180次
     -m          生成的报告文件的存放目录
```

##### nmon analyser下载

[nmon_analyser_v66](http://nmon.sourceforge.net/pmwiki.php?n=Site.Nmon-Analyser)

#### 官网及其他参考

[nmon官网](http://nmon.sourceforge.net/pmwiki.php?n=Main.HomePage)