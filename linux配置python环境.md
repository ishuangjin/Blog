# linux配置python环境

## 一、安装python3

虽然centos7系统默认安装了python2，但是目前python一般使用python3较多，所以需要安装下python3

1. 执行命令`whereis python`查看下是否安装了python3，如果已经安装好了python3可以直接跳到后面改系统默认python和pip
2. 如果没有安装python3执行以下命令进行下载

    ```bash
    wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
    ```

    也可以从[python官网](https://www.python.org/downloads/source/)进行下载后上传到服务器
    > `rz`执行上传命令
    > `sz`执行下载命令
    > 如果提示没有安装，可以用`yum install lrzsz`命令安装该工具

3. 上传成功后执行`tar xzvf Python-3.6.5.tgz`命令解压
4. 准备编译环境

    ```bash
    yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
    ```

5. 编译安装
    执行`cd Python-3.6.5`进入解压后的目录下，依次执行如下三个命令：

    ```linux
    ./configure --prefix=/root/app/Python-3.6.5
    make
    make install
    ```

    > 其中--prefix是Python的安装目录

6. 安装成功后会提示`Successfully`

## 二、更改系统默认python和pip

通过软链接进行更改

1. 使用`python -V`查看系统当前默认python版本，我这里是`Python 2.7.5`，默认的是python2，现在把它改成默认python3
2. 使用`whereis pytohn`查看python安装地址，我这里的地址是`/usr/bin/`
3. 备份原来的软链接

    ```linux
    mv /usr/bin/python /usr/bin/python.bak
    ```

4. 创建python3的软链接

    ```linux
    ln -s /usr/bin/python3 /usr/bin/python
    ```

5. `python -V`验证下是否成功，此时使用`python2 -V`可以看到python2的版本，所以python2也是可以使用的，如果不行可以把之前备份的那个软链接重命名成python2

6. 同样方法更改系统默认pip为pip3

    ```linux
    ln -s /usr/bin/pip3 /usr/bin/pip
    ```

## 三、更改国内pip源并更新pip版本

默认pip源有的时候速度比较慢，我们更改成国内pip源，这里以阿里源为例

1. 找到`pip.conf`文件，执行`ll ~/.pip/`命令看是否存在
2. 不存在则新建

    ```linux
    touch ~/.pip/pip.conf
    ```

3. 在上述文件中添加或修改:

    ```linux
    tee ~/.pip/pip.conf <<-'EOF'
    [global]
    index-url = https://mirrors.aliyun.com/pypi/simple/

    [install]
    trusted-host=mirrors.aliyun.com
    EOF
    ```

4. 更新pip版本

    ```linux
    python -m pip install --upgrade pip
    ```

5. 常用的pip源

    > pypi 清华大学源：<https://pypi.tuna.tsinghua.edu.cn/simple>
    > pypi 豆瓣源 ：<http://pypi.douban.com/simple/>
    > pypi 腾讯源：<http://mirrors.cloud.tencent.com/pypi/simple>
    > pypi 阿里源：<https://mirrors.aliyun.com/pypi/>

## 四、修改yum配置的python版本

- yum需要依赖python2环境，如果python3.6并作为默认编译器，则会报错`File "/usr/bin/yum", line 30 except KeyboardInterrupt, e:`，所以需要更改`/usr/bin/yum`文件中的第一行为`#!/usr/bin/python2`
- 随后可能出现错误`：File "/usr/libexec/urlgrabber-ext-down", line 28`，同理，打开`/usr/libexec/urlgrabber-ext-down`，修改第一行为`#!/usr/bin/python2`

## 五、参考

> pypi官方主页：<https://pypi.org/>
> 阿里开发者社区：<https://developer.aliyun.com/mirror/pypi>
> Linux系统安装Python3环境：<https://blog.csdn.net/HD243608836/article/details/121417965>
> Linux 中把Python3设为默认Python版本的几种方法：<https://blog.csdn.net/qq_33261400/article/details/107520660>
