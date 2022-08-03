## yum介绍

yum是centos中用来管理软件包的工具，可以自动安装软件的相关依赖，但是卸载时会自动卸载软件的依赖包，所以卸载时需要注意这些依赖是否会影响到其他软件或者是否会导致系统奔溃，**谨慎使用`yum uninstall`功能，如果可以请尽量使用`yum remove`代替**

## 为什么要配置yum源

默认yum源地址是海外地址，访问速度较慢，配置成国内镜像源可以大大提高访问速度

> 什么是repo文件？
> repo 文件是 Linux 中yum源（软件仓库）的配置文件，通常一个 repo 文件定义了一个或者多个软件仓库的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取和应用！

## 配置方法

### 备份`CentOS-Base.repo`文件

```linux
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```
### 下载新的`CentOS-Base.repo`到`/etc/yum.repos.d/`

    *根据自己系统选择使用的源文件，查看CentOs发行版本命令：`cat /etc/redhat-release`
    我这里系统是`CentOS Linux release 7.6.1810 (Core)`，使用如下命令*
```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```
或者
```bash
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```
其他镜像源更改命令中的网址即可，eg：使用网易镜像源命令如下
```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
```
### 解决阿里源生成缓存报错

因为用的不是阿里云ESC机器，直接生成缓存会报错：`Couldn't resolve host 'mirrors.cloud.aliyuncs.com'`，虽然不影响使用，但还是改一下相关配置

```bash
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
```
### 运行`yum makecache`生成缓存

### 运行`yum repolist`查看结果

### 运行`yum -y update`更新

## 国内yum源

> - 网易163：http://mirrors.163.com/.help/
> - 中科大：https://mirrors.ustc.edu.cn/help/
> - sohu：http://mirrors.sohu.com/help/
> - 阿里源：https://opsx.alibaba.com/mirror
> - 清华大学：https://mirrors.tuna.tsinghua.edu.cn/
> - 浙江大学：http://mirrors.zju.edu.cn/
> - 中国科技大学：http://centos.ustc.edu.cn/

## 参考链接

> 阿里云开发者社区：https://developer.aliyun.com/mirror/centos