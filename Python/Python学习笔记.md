# 学习python笔记
python是一门解释型、弱类型的语言

变量和命名规则
变量的概念： 
变量:容器,放程序中的数据
```py
money = 100
```
# 变量和常量
* 变量：值可以发生改变  
* 常量：固定的值，不能发生改变  
* 变量：声明变量实际上就是给内存要空间
* 变量里可以存放的类型有哪些？
    * 字符串
    * 整形
    * 浮点数
    * boolean
    * list列表
    * dict 字典
    * set 列表（无重复）
使用type()可以查看类型  
变量命名规则： 
* 标识符： 字母，数字，下划线组成，⚠️不能以数字开头  
* 严格区分大小写 
* 不能使用python关键字  
类：每个单词的首字母大写 比如 GetName  
python推荐使用下划线命名法  
```py
pay_money
get_name
 ```

```py
# 查看方法
import keyword
print(keyword.kwlist)
['and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'exec', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'not', 'or', 'pass', 'print', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

## 常量
全大写的名字 = 常量
```py
NAME = '老王'
# 这个NAME就代表是常量
```

# print
print(xx,xx,xx,sep='这里是分隔符,默认是空格')  
print(xxx,end='')
print(r'xxxxxxx')
keyword|解释|默认值
:---:|:--:|:--:
sep|分隔符|空格
end|结果值最后|\n(换行)
r|原样输出字符串内容,忽略转义字符|print(r'xxxxx')


## 转义字符
字符|解释
|:--:|:--:|
\n|换行
\t|制表符(空格)
\'|'
\""|"
\r|回车
两个\ | \
解释:
\n 和\r都是特殊控制符,这些都来自于老式电传打字机的功能  
\n是newline 开个新行
\r是Carriage return 打印头回到行首.如果没有\n就直接\r,那么这行就会被覆盖打印了,现在各个操作系统上处理不太一样,在不同终端显示上也不太相同  
zaiidle中是不能实现\r的功能的  
意思就是说,,一切以PyCharm为准,IDLE里实现不了\r的功能  
而\r的功能是让光标回到行首,覆盖之前的内容,所以就产生了 "我是Python" 覆盖了 "你好!" 的结果  

## 三引号
三引号会保留里面的格式不会变
* 保留格式的字符串
* 注释
```py
# 输出
print('''【滴滴出行】验证码：(123456)，
您正在使用短信验证码登录功能，5分钟内有效。
转发可能导致帐号被盗，请勿泄露给他人!''')
# 注释
'''
我是一个莫得感情的
多行注释
hhhh
'''
```
输出拼接  
```py
person = '小明'
address = '辽宁省沈阳市xx广场'
phone = 13456778900
print('收件人:%s,地址:%s,联系电话:%s' % (person, address, phone))
```

格式化输出
格式化输出|解释
|:--:|:--:|
%s|字符串
%d|数字(整型) => int(xx) 取整 99.99 => 99
%f|浮点型(小数点后面的位数,四舍五入)

%.1f => 小数点后1位四舍五入
```py
money = 123.456789
print('money%.5f' % money) 
# money123.45679 会四舍五入
```
强制类型转换
方法|解释
|:--:|:--:|
str(int)|把数字转为字符串 int=>str

# Python常见中英对照表
英文    |   解释
---|:--:|
print | 打印  |
Defined|定义
SyntaxError|语法错误
NameError|名字错误
Invalid|无效的
Character|字符 
Help|帮助
Function|函数
Built-in(builtins)|内置
Module|模块
Value|值
Stream|流
Default|默认的
Format|格式化
Digit|数字