### 一、使用步骤

#### 1. 首先导入该模块
`import argparse`

#### 2. 然后创建一个解析对象
`parser = argparse.ArgumentParser()`

#### 3. 向该对象中添加你要关注的命令行参数和选项，每一个add_argument方法对应一个你要关注的参数或选项
`parser.add_argument()`

#### 4. 调用parse_args()方法进行解析
`parser.parse_args()`

### 二、实例

#### 1. 编写代码如下

```python
# 步骤一
import argparse


def parse_args():
    description = "TCT任务控制台"
    # 步骤二
    parser = argparse.ArgumentParser(
        description=description)
    # 这些参数都有默认值，当调用parser.print_help()或者运行程序时由于参数不正确(此时python解释器其实也是调用了pring_help()方法)时，会打印这些描述信息，一般只需要传递description参数，如上。
    # 步骤三，后面的help是我的描述
    help = 'task/flow(类型：任务/工作流)'
    parser.add_argument('type', type=str, help=help)
    parser.add_argument('--count', default=0)
    # 步骤四
    args = parser.parse_args()
    return args


if __name__ == '__main__':
    args = parse_args()
    print(args.type)
    print(args.count)  # 直接这么获取即可。
```

#### 2. 查看帮助

```bash
$ python test.py -h
usage: test.py [-h] [--count COUNT] type

TCT任务控制台

positional arguments:
  type           task/flow(类型：任务/工作流)

optional arguments:
  -h, --help     show this help message and exit
  --count COUNT
```

#### 3. 根据帮助执行程序结果如下

```bash
$ python test.py www
www
0
$ python test.py ccc --count 12
ccc
12
```

### 三参考

> [python学习之argparse模块：][1]https://zhuanlan.zhihu.com/p/28871131

