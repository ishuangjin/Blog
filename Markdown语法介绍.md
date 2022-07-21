# Markdown语法介绍

## 一、 什么是Markdown

Markdown是一种*轻量级标记语言*，通过简单的标记语法，它允许人们 “使用易读易写的纯文本格式编写文档，然后转换成有效的 HTML 文档。”



## 二、 Markdown语法

#### 1. 标题

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题



#### 2. 列表

```markdown
+ 无序列表一
	- 无序列表二
		* 无序列表三
```
+ 无序列表一
	- 无序列表二
		* 无序列表三

```markdown
1. 有序列表一
	1. 有序列表1
2. 有序列表二
```
1. 有序列表一
  1. 有序列表1
2. 有序列表二




#### 3.分割线

```markdown
---
***
```

---
***



#### 4. 斜体和粗体

```markdown
*斜体*
```
*斜体*
```markdown
**粗体**
```
**粗体**
```markdown
~~删除线~~
```
~~删除线~~



#### 5. 超链接和图片
```markdown
[百度](www.baidu.com)
```
[百度](www.baidu.com)

```markdown
![百度logo](https://www.baidu.com/img/flexible/logo/pc/result.png)
```
![百度logo](https://www.baidu.com/img/flexible/logo/pc/result.png)



#### 6. 文字引用

```markdown
>引用层级一
>>引用层级二
>引用层级二
>>>引用层级三
>>> 
>引用层级一
```
>引用层级一
>>引用层级二
>引用层级二
>>>引用层级三
>
>引用层级一



#### 7. 行内代码块

```markdown
`行内代码块`
```
`行内代码块`



#### 8. 代码块

```markdown
1. 空余四个空格缩进表示代码块
2. 三个\`表示(\用以转义)
\```python
if True:
    print("Hello")
\```
```

```python
if True:
    print("Hello")
```
