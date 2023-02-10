# configparser

## 一、config.ini解析

```ini
[config]
sleep=true
language=zh

[session1]
option1=1
option2=2023
option3=3.14

[session2]
option4=session2_option4

```

## 二、读取配置文件，read_config.py解析

```python
import configparser

# -实例化configParser对象
config = configparser.ConfigParser()
# -read读取ini文件
config.read(r'D:\Github\MyScript\baidu翻译api\config\config1.ini', encoding='GB18030')
# -sections得到所有的section，并以列表的形式返回
print('sections:' , ' ' , config.sections())

# -options(section)得到该section的所有option
print('options:' ,' ' , config.options('config'))

# -items（section）得到该section的所有键值对
print('items:' ,' ' ,config.items('session1'))

# -get(section,option)得到section中option的值，返回为string类型
print('get:' ,' ' , config.get('session2', 'option4'))

# -getint(section,option)得到section中的option的值，返回为int类型
print('getint:' ,' ' ,config.getint('session1', 'option2'))
print('getfloat:' ,' ' , config.getfloat('session1', 'option3'))
print('getboolean:' ,'  ', config.getboolean('session1', 'option1'))
"""
首先得到配置文件的所有分组，然后根据分组逐一展示所有
"""
for sections in config.sections():
    for items in config.items(sections):
        print(items)
```

以上代码运行结果

```bash
$ C:/Users/anchnet/AppData/Local/Programs/Python/Python37/python.exe d:/Github/MyScript/test.py
sections:   ['config', 'session1', 'session2']
options:   ['sleep', 'language']
items:   [('option1', '1'), ('option2', '2023'), ('option3', '3.14')]
get:   session2_option4
getint:   2023
getfloat:   3.14
getboolean:    True
('sleep', 'true')
('language', 'zh')
('option1', '1')
('option2', '2023')
('option3', '3.14')
('option4', 'session2_option4')
```

## 三、写入配置文件，writer_config.py解析

```python
import configparser

# -实例化configParser对象
config = configparser.ConfigParser()
# -read读取ini文件
config.read(r'D:\Github\MyScript\baidu翻译api\config\config1.ini', encoding='GB18030')
list = []
list = config.sections()  # 获取到配置文件中所有分组名称
if 'type' not in list:  # 如果分组type不存在则插入type分组
    config.add_section('type')
    config.set('type', 'stuno', '10211201')# 给type分组设置值

# config.remove_option('type', 'stuno')  # 删除type分组的stuno
config.remove_section('type')  # 删除配置文件中type分组
o = open(r'D:\Github\MyScript\baidu翻译api\config\config1.ini', 'w')
config.write(o)
o.close()  # 不要忘记关闭
```

## 参考

> <https://blog.csdn.net/songlh1234/article/details/83316468>
