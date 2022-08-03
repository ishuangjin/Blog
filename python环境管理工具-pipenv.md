# python环境管理工具

[toc]

## Pipenv的作用

可以利用pipenv来实现同时管理项目中的python虚拟环境和相关包依赖。

## 使用步骤

1. cmd中输入命令 pip install pipenv 安装pipenv
2. cmd切换路径到需要建立虚拟环境的项目目录下
3. 输入命令 pipenv install 开始安装虚拟环境
4. 安装完毕后输入命令 pipenv shell 进入虚拟环境

## Pipfile文件

- 虚拟环境安装完毕后项目目录中将创建Pipfile/Pipfile.lock两个文件
- Pipfile：用于保存项目的python版本、依赖包等相关信息
- Pipfile.lock：用于对Pipfile的锁定
- Pipfile文件可以单独移放到其他项目内，用于项目虚拟环境的建立和依赖包的安装

## 常用命令

- pipenv install：

  - 若项目目录中虚拟环境未创建且无Pipfile文件，将安装虚拟环境并创建Pipfile文件
  - 若项目目录中虚拟环境未创建且有Pipfile文件，将根据Pipfile文件来安装相应python版本和依赖包
  - 若项目目录中虚拟环境已创建且有Pipfile文件，将根据Pipfile文件来安装依赖包
- pipenv install xx:：安装python包
- pipenv uninstall xx:：卸载python包
- pipenv shell：进入虚拟环境(项目目录下)
- exit：退出虚拟环境
- pipenv graph：显示包依赖关系
- pipenv --venv：显示虚拟环境安装路径

## 参考

> [https://zhuanlan.zhihu.com/p/349919589](https://zhuanlan.zhihu.com/p/349919589)
