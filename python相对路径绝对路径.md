# python使用相对路径导入模块

## os.path.abspath(__file__)

os.path.dirname(__file__)：返回脚本的绝对路径

## os.path.dirname()

os.path.dirname()：返回当前脚本文件的所在路径（上一层目录）

## os.path.join()

os.path.join()：拼接路径，

## 上面组合使用

### 1.返回当前脚本文件的所在路径（上一层目录）

main_path = os.path.dirname(os.path.abspath(__file__))

### 2.拼接路径

test1_path = os.path.join(main_path, "mod_a\\test1.py")

### 3. 在sys.path中添加import搜索的路径

sys.path.append(main_path)

## 参考

> [Python 标准库 » 文件和目录访问 » os.path --- 常用路径操作](https://docs.python.org/zh-cn/3/library/os.path.html)
