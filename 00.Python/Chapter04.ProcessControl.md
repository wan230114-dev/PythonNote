# 4. 流程控制
## 逻辑语句
- 学习先导
  - 何为语句与表达式？见第一章[语句和表达式](/00.Python/Chapter01.PythonReview#_1411-语句和表达式)
  - 什么是一个完整的语句/语句块？
  - 本章学习的语句：
    - if-elif-else语句：用于判断语句块的执行与否
    - while语句：用于无限循环的逻辑执行
    - for语句：用于<[可迭代对象](/00.Python/Chapter05.DataTraversal#_53-可迭代对象)>的循环（字符串，range()返回对象）
    - 循环控制语句
      - break语句：用于终止当前语句块的循环
      - continue语句：用于开始一次当前语句块的新循环

注：布尔值可以直接以True和False代替
### 5.1.1. if-elif-else语句
- 作用：选择性执行语句。
- if-else常规语句
  - 语法：
    ```python
    if 真值表达式:
        语句1
    else:
        语句块2
    ```
    - 运行机理：本质判断是，`if 布尔值`，实际中运行的是`if bool(obj)`，比如`if 100`为`if bool(100)`  

- if-elif-else多条件判断语句
  - 语法：
    ```python
    if 真值表达式:
        语句1
    elif 真值表达式2:
        语句2
    elif 真值表达式3:
        语句3
    ...
    else:
        语句块4
    ```

- 条件表达式（三元表达式）
  - 作用：同样进行执行控制，但更灵活简洁
  - 语法：
    ```python
    <表达式1> if <真值表达式> else <表达式2>
    ```
  - 返回：若真值表达式为真，则返回表达式1结果，否则返回表达式2结果  
    `<表达式1，为真返回> if <真值表达式> else <表达式2，为假返回>`
    - 注意：
      - `表达式1`和`表达式2`返回的是表达式计算完毕的结果。
        示例：
```python
L = [1, 2, 3]
i = 4
a = L[0] if True else L[i]
```

注意：表达式不是语句，表达式能赋值给变量，语句则不能。此在第一章[语句和表达式](/00.Python/Chapter01.PythonReview#_1411-语句和表达式)已有详细描述。

---
示例1：
```python
a = 25
if 0 <= a < 18:
    print('未成年')
elif a <30:
    print('年轻人')
else:
    print('不再年轻')
```
运行结果：
```
年轻人
```

示例2：
```python
# 条件表达式
print('a' if True else 'b')
print('a' if False else 'b')

# 使用条件表达式计算绝对值
n = -20
m = n if n > 0 else -n
print(m)
```
运行结果
```
a
b
20
```

### 5.1.2. while语句
格式：
```python
while <真值表达式1>：
	语句1
else <真值表达式2>：
	语句2
```
运行机制：
> - 第一行while一行中，计算判断<真值表达式1>。  
> - 若为True，则执行完后面语句后又跳回while一行，重新计算判断<真值表达式1>。
> - 若为False，则执行else语句，else语句可以省略。

---

示例：
```python
a = 0
while a < 6:
    print(a, end=' ')
    a += 1
# 0 1 2 3 4 5
```

### 5.1.3. for语句
- 作用：循环遍历，返回可迭代“对象”元素
- 格式：
    ```python
    for <变量列表> in <可迭代对象>：
        语句块1
    else:
        语句块2
    ```
    - `<变量列表>`可以直接用多个变量接收，相当于第一章中几种赋值方式中的多重赋值
- 常用级联函数: `range()`

---

示例：
```python
for a in [(0, '001'), (1, '002'), (2, '003')]:
    print('a:', a)

print('-'*13)
for n, a in [(0, '001'), (1, '002'), (2, '003')]:
    print('n:', n, ', a:', a)

print('-'*13)
for i in range(3, 6):
    print(i, end=' ')
```

运行结果：
```
a: (0, '001')
a: (1, '002')
a: (2, '003')
-------------
n: 0 , a: 001
n: 1 , a: 002
n: 2 , a: 003
-------------
3 4 5
```

#### 机制探索：
```python
for <变量列表> in <可迭代对象>：
    语句块1
else:
    语句块2
```

1. `for ... in ...`语句中的`in`不同于操作运算符`in`。jump_for操作运算符`in`是判断是否存在于某个数据容器或可迭代对象，这里的in是指在当前这个可迭代对象中逐个取值。
2. `for ... in <可迭代对象>`的取值是按照`<可迭代对象>`索引来取值的。`<可迭代对象>`内的数据可以发生改变。但长度变化不同，对于序列，长度可以改变，对于集合，如果长度发生变化将导致触发异常。对于可变序列，如列表，若循环中长度一直增加，将有可能造成无限循环。
3. `for`语句块本质上是用while、迭代器、next()以及try捕获异常实现的，详细介绍见下一章[迭代器之for语句的本质](/00.Python/Chapter05.DataTraversal#jump_for)

---

示例1：探索`for ... in ...`中`in`的含义  
code1:
```python
print('for循环探索')
i = 6
for x in range(1, i):
    # for ... in ... 迭代[1,i)
    # 每个循环向后逐个取出元素，赋值给x
    print('x=', x, '  i=', i)
    i -= 1

print('while循环探索')
i = 6
x = 1
while x in range(1, i):
    # ... in ... 判断x是否在[1,i)内
    print('x=', x, '  i=', i)
    x += 1
    i -= 1
```
运行结果：
```
for循环探索
x = 1     i = 6
x = 2     i = 5
x = 3     i = 4
x = 4     i = 3
x = 5     i = 2
while循环探索
x = 1     i = 6
x = 2     i = 5
x = 3     i = 4
```
code2: in的正确使用示例
```python
print('for循环探索')
i = 6
it = range(1, i)
for x in it:
    if x in it:  # in判断操作，判断x是否在[1,i)内
        print('x=', x, '  i=', i)
        i -= 1
        it = range(1, i)
    else:
        break

print('while循环探索')
i = 6
x = 1
while x in range(1, i):  # in判断操作，判断x是否在[1,i)内
    print('x=', x, '  i=', i)
    x += 1
    i -= 1
```
运行结果：
```
for循环探索
x= 1   i= 6
x= 2   i= 5
x= 3   i= 4
while循环探索
x= 1   i= 6
x= 2   i= 5
x= 3   i= 4
```


示例2：探索`for <变量列表> in <可迭代对象>`中`<可迭代对象>`变化带来的影响  
code1: 
```python
L = [1, 2, 3]
for x in L:
    print(x, L, end=' --> ')
    L.remove(x)  # 此行注释将导致无限循环
    L.append(x+10)
    print(L)
```

```
1 [1, 2, 3] --> [2, 3, 11]
3 [2, 3, 11] --> [2, 11, 13]
13 [2, 11, 13] --> [2, 11, 23]
```
code2: 取值
```python
s ={1, 2, 3}
for x in s:
    print(x, s, end=' --> ')
    s.remove(x)
    s.add(x+10)
    print(s)
```
运行结果：
```
1 {1, 2, 3} --> {11, 2, 3}
2 {11, 2, 3} --> {3, 11, 12}
3 {3, 11, 12} --> {11, 12, 13}
11 {11, 12, 13} --> {21, 12, 13}
12 {21, 12, 13} --> {21, 22, 13}
13 {21, 22, 13} --> {21, 22, 23}
```

code3: 取值遗失3, 13
```python
s ={1, 2, 3}
for x in s:
    print(x, s, end=' --> ')
    s.remove(x)
    s.update({x+10})
    print(s)
    # 但集合中不能更改s长度，否则触发RuntimeError异常，如
    # s.update({x+10, x+100})
```
```
1 {1, 2, 3} --> {11, 2, 3}
2 {11, 2, 3} --> {3, 11, 12}
11 {3, 11, 12} --> {3, 12, 21}
12 {3, 12, 21} --> {3, 21, 22}
21 {3, 21, 22} --> {3, 22, 31}
22 {3, 22, 31} --> {32, 3, 31}
31 {32, 3, 31} --> {32, 41, 3}
```

#### 5.1.3.1. 常见联用函数：range()函数
- 格式：
  - `range(s, e, step)`, s--开始数值, e--结束数值(不包含), step--步数(可为负数)
- 规则类似于序列操作中的切片
- 返回：返回数值序列的“迭代器对象”, 非字符串（序列）

```python
it = range(3, 6)
print(it)  # range(3, 6)
for i in range(3, 6):
    print(i, end=' ')
print('\n---------------')
print(list(range(3, 8)))      # 3,4,5,6,7
print(list(range(3, 8, 2)))   # 3,5,7
print(list(range(3, 0)))      # 无
print(list(range(3, 0, -1)))  # 3,2,1
```
运行结果：
```
range(3, 6)
3 4 5
---------------
[3, 4, 5, 6, 7]
[3, 5, 7]
[]
[3, 2, 1]
```


数据集合与优化【？】

5.1.4  	pass语句
填充语法空白
如：if语句中的使用
if Ture:
pass
else:
print('输入有问题')
5.1.5  	break语句
退出当前语句块

注意1：不干扰外层其他语句块
如：
for i in range(2):
    print(i)
    for j in ['a', 'b', 'c']:
        print('j:', j)
        if j == 'b':
            break
'''
0
j: a
j: b
1
j: a
j: b
'''
注意2：在for-else和while-else语句中break是退出整个语句块：
如：
for-else中else的语句被跳过：
for i in [1,2,3,4]:
    if i==2:
        continue
    print(i)
else:
    print("hehe")
结果为
1
3
4
hehe
	for i in [1,2,3,4]:
    if i==2:
        break
    print(i)
else:
    print("hehe")
结果为
1


5.1.6  	continue语句
问题引入：如何让程序不再向下执行，重新开始一次新的循环？
说明：
	for循环中迭代取下一个元素。
	while语句中，真值表达式中会重新判断条件。

5.1.7  	其他语句
5.1.7.1	语句嵌套
一个语句嵌套到另一个语句内部
5.1.7.2	布尔运算
返回：布尔值或对象
运算符：not、and、or
布尔非操作：not x
>>> not True
False

布尔与操作：x and y
机理：从左到右运算，只要有一个不满足就返回x值，否则返回y
优先返回假值对象。
先计算x，若x为False，返回x，否则返回y值。
使用对象表示真假，不同于C/C++的“&”【？】（？？？）。
>>> 0 and 20		#优先返回假值对象
0
>>> 20 and 0
0

>>> 0 and 20		#若x为False，返回x，否则返回y值。
0
>>> 10 and 10
10
>>> 10 and 20
20
>>> 20 and 10
10

布尔或操作：x or y
机理：从左到右运算，只要有一个满足就返回x值，否则返回y
优先返回真值对象。
先计算x，若x为True，返回x，否则返回y值。
>>> 0 or 20		#优先返回真值对象
20
>>> 20 or 0
20

>>> 0 or 20		#若x为True，返回x，否则返回y值。
20
>>> 10 or 10
10
>>> 10 or 20
10

>>> 1 or 20
1
>>> 20 or 1
20
备注：这个过程完全满足逻辑判断。如
>>> 6>5 and 6>3
True
>>> 6>5 and 7
7
5.1.7.3	正负号运算符
+（正号）
-（负号）
一元运算符
语法：+/- 表达式