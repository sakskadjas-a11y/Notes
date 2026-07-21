# python

## 基础语法

### 一.标识符

1. 第一个字符为字母或_
2. 大小写敏感。

### 二.关键字

True 布尔真值；False 布尔假值；None 空值或无值；

<img src="C:\Users\aksura\Pictures\Screenshots\屏幕截图 2026-07-15 175836.png" style="zoom: 67%;" />

### 三.注释

```python
#单行注释 快捷键ctrl+/
多行注释
注释为ctrl+/
取消为ctrl+win+/
```

### 四.数字类型

int 整数；bool（布尔）：true false none；float 浮点；complex 复数 1+2j

### 五.字符串

1.反斜杠可以用来转义，使用 **r** 可以让反斜杠不发生转义。

```python
print('hello\nrunoob')      # 使用反斜杠(\)+n转义特殊字符
print(r'hello\nrunoob')     # 在字符串前面添加一个 r，表示原始字符串，不会发生转义
输出结果
hello
runoob
hello\nrunoob
```

2.

```python
str='123456789'
 
print(str)                 # 输出字符串
print(str[0:-1])           # 输出第一个到倒数第二个的所有字符
print(str[0])              # 输出字符串第一个字符
print(str[2:5])            # 输出从第三个开始到第六个的字符（不包含）
print(str[2:])             # 输出从第三个开始后的所有字符
print(str[1:5:2])          # 输出从第二个开始到第五个且每隔一个的字符（步长为2）
print(str * 2)             # 输出字符串两次
print(str + '你好')         # 连接字符串
```



### 六.多个代码构成代码组

复合语句首行以关键字开始，以：结束

```python
if expression : 
   suite
elif expression : 
   suite 
else : 
   suite
```

### 七.print输出

**print** 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end=""**：

```python
# 换行输出
print( x )
# 不换行输出
print( x, end="" )
```

### 八.导入其他模块

在 python 用 **import** 或者 **from...import** 来导入相应的模块。

将整个模块(somemodule)导入，格式为： **import somemodule**

从某个模块中导入某个函数,格式为： **from somemodule import somefunction**

从某个模块中导入多个函数,格式为： **from somemodule import firstfunc, secondfunc, thirdfunc**

将某个模块中的全部函数导入，格式为：from somemodule import 

## 基本数据类型

### 一.多变量

```python
查看数据类型
print(type(x))
```

