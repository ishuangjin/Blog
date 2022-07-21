#### 简介

使用pymysql能够实现使用python连接数据库进行增删改查

#### 安装

```
pip install pymysql
```

#### 使用

```python
#!usr/bin/python3
#coding-utf-8

import pymysql


# 数据库配置信息
db_cfg = {'host': '192.168.77.88',
              'database': 'task_schedule',
              'user': 'root',
              'password': '123456'}
              
# 创建数据库连接
db = pymysql.connect(host=db_cfg['host'], database=db_cfg['database'],
                     user=db_cfg['user'], password=db_cfg['password'], charset='utf8mb4')
                     
# 创建游标
cursor = db.cursor()

# sql语句
sql = """SELECT * FROM STUDENT;"""

# 执行sql语句
try:
    cursor.execute(sql)
    sql_results = cursor.fetchall()
except:
    import traceback
    traceback.print_exc()
    print("Error: unable to fetch data")
# 关闭数据库
finally：
    db.close()
```

#### 使用导出的sql文件

因为实际使用环境不能导入pymsql模块，只能把查询结果导出为txt文档，再进行转换为相同的元组格式

##### 导出查询结果

```mysql
mysql -h <ip> -uroot -p<passwd> -e "<sql语句>" > test.txt
```

##### 结果转化为元组

```python
def get_task(sql_txt):
    task_list = []
    with open(sql_txt, 'r') as f:
        lines = f.readlines()
        for line in lines:
            task_line = line.split()
            task_id = task_line[0]
            task_name = task_line[1]
            task_tuple = (task_id, task_name)
            task_list.append(task_tuple)
        task_tuple = tuple(task_list[1:])
    return task_tuple
```

#### 参考

[Python+MySQL数据库操作（PyMySQL）](https://www.yiibai.com/python/python_database_access.html)