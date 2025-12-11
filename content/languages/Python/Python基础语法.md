---
title: "Python基础语法"
summary: "基础操作，容器数据类型，以及函数"
tags: ["Python"]
categories: ["languages"]
created: 2025-12-11 09:00
draft: "false"
showToc: "true"
---
# 基础操作

## 类型和类型转换

### 类型的判断

Python 中所有类型都是对象
- 可以使用type() 来查看变量类型，使用isinstance() 来判断变量类型
	- type() 不会认为子类是一种父类类型
	- isinstance() 会认为子类是一种父类类型

```python
num1 = 1  
bool1 = True  
print(f'{type(num1)}\n{type(bool1)}')  
print(f'bool is int ? {isinstance(bool1,type(num1))}') # bool 是 int 的子类

# <class 'int'>
# <class 'bool'>
# bool is int ? True
```

### 类型的种类

Python 的内建数据类型
- 数字：int ，float ，complex
- 布尔：bool
- 字符串：str
- 序列：list ，tuple ，range
- 集合：set ，frozenset
- 映射：dict
- NoneType：None

#### 整数类型

##### 内部原理

Python 的 int 是**自动扩容**的大整数，大小只受限于可用内存

Python 的整数在内部使用**数组存储**多个数字块(类似大数运算)    ：
- 小整数（一个机器字长内）：直接存储，性能接近C语言的int
- 大整数：用数组存储多个"数字块"，每个块通常是30位（CPython）
	- ex 12345678901234567890 可能存储为：`[67890, 45678, 12390, 1]`（简化示意）
- 运算时按照"多位数加减乘除"的算法处理
##### 整数池机制

Python中存在整数池机制，CPython 预先创建了常用小整数对象池`[-5 到 256]`，相同值的小整数共享同一个对象。

```python
# 小整数：共享对象 
a = 100 
b = 100 
print(a is b) # True 
print(id(a) == id(b)) # True，同一内存地址 

# 大整数：不保证共享 
c = 1000 
d = 1000 
print(c is d) # 可能是 False 
print(c == d) # True
```

1. 使用 ` == ` 而非 `is` 比较整数数值
2. `is` 比较的是对象身份（内存地址），不是值
#### 布尔类型

bool 是 int 的 子类，`True` 和 `False` 本质上是整数 1 和 0

```python
print(isinstance(True, int)) # True
print(isinstance(False, int)) # True
print(True == 1) # True
print(False == 0) # True 

# 可以参与数值运算 
print(True + True) # 2 
print(True * 5) # 5 
print(sum([True, False, True])) # 2

num0 = 0  
num1 = 1  
num2 = 2  
print(num2 or num0) # 2  
print((num1 and num2)*5) # 10
```

在布尔上下文中（如 if 语句），以下值被视为 False： 
- 数值零：`0`, `0.0`, `0j`（复数零）
- 空容器：`""`, `[]`, `()`, `{}`, `set()`
- 特殊值：`None`, `False`
**除此之外的所有值都视为 True**

#### String类型

字符串是不可变的序列类型

也存在字符串驻留机制（intern），Python 通过 intern 机制让相同的字符串共享内存，提高性能。

CPython实现细节，自动驻留：
- 代码中的字符串字面量
- 长度为0和1的字符串
- 符合标识符规则的字符串（字母、数字、下划线）
- 编译期能确定的字符串常量

```python
# 字面量会自动驻留 
a = "hello" 
b = "hello" 
print(a is b) # True 

# 包含空格的字面量也会驻留（Python 3.7+） 
c = "hello world"
d = "hello world" 
print(c is d) # True 

# 运行时生成的字符串不保证驻留 
e = "".join(["h", "e", "l", "l", "o"]) 
f = "hello" 
print(e is f) # False 
print(e == f) # True 

# 动态拼接不驻留 
x = "hel" + "lo" 
y = "hello" 
print(x is y) # 可能True可能False（取决于优化） 

# 可以手动驻留 
import sys 
e_interned = sys.intern(e) 
print(e_interned is f) # 可能True（如果f已被驻留）
```

### 类型转换

#### 基本类型转换

**int() - 转为整数**

```python
# 参数必须是数字，字母也是进制中的数字
int("123")        # 123
int(3.9)          # 3，截断不四舍五入
round(3.9)        # 4，四舍五入用round()
int("ff", 16)     # 255，支持2-36进制
int(True)         # 1
```

注意：

```python
int("12.3")       # ValueError，不能直接转带小数点的
```

**float() - 转为浮点数**
```python
float("3.14")     # 3.14
float(10)         # 10.0
float("1e3")      # 1000.0，支持科学计数法
float("inf")      # inf，无穷大
```

**str() - 转为字符串**
```python
str(123)          # "123"
str([1, 2])       # "[1, 2]"
str(None)         # "None"
# 任何类型都可转换
```

**bool() - 转为布尔值**
```python
bool(0)           # False
bool("")          # False
bool([])          # False
bool("0")         # True，非空字符串
bool([0])         # True，非空列表
# 规则：0/空容器/None为False，其他非空为True
```

#### 字符和ASCII码转换
```python
ord('A')          # 65，字符→ASCII码
ord('a')          # 97
chr(65)           # 'A'，ASCII码→字符
chr(97)           # 'a'
```

#### 进制转换
```python
# 整数→其他进制字符串
bin(10)           # '0b1010'，二进制
oct(10)           # '0o12'，八进制
hex(10)           # '0xa'，十六进制

# 其他进制字符串→整数
int("1010", 2)    # 10，二进制→十进制
int("12", 8)      # 10，八进制→十进制
int("a", 16)      # 10，十六进制→十进制
```

## 运算符和表达式

- 幂运算： `**`
	- `print(2 ** 0.5)`
- 除法运算：`/` 直接除法运算，int转为float
- 整除： `//` 向下取整
- ` == `：比较的是数值
- **身份运算符** `is` / `is not`
	- 比较两个对象是否是同一个对象（内存地址相同），不是比较值
- **成员运算符 **`in` / `not in`
	- 检查元素是否在容器中
	- list/tuple: O(n)
	- set/dict: O(1)
- **逻辑运算符**返回特值
	- 与Java不同，Python的逻辑运算符不返回布尔值，而是返回实际的对象
 
 ```python
# and：返回第一个假值，或最后一个值
print(0 and 5)           # 0，第一个是假值
print(3 and 5)           # 5，都为真，返回最后一个
print(3 and 0 and 5)     # 0，第一个假值

# or：返回第一个真值，或最后一个值
print(0 or 5)            # 5，第一个真值
print(3 or 5)            # 3，第一个真值
print(0 or "" or 5)      # 5，第一个真值
print(0 or "" or [])     # []，都为假，返回最后一个

# 实际应用：设置默认值
name = user_input or "Guest"        # 如果输入为空，使用默认值
result = cache.get(key) or compute()  # 缓存未命中时计算

# 短路求值
x = 0
result = x != 0 and 10 / x  # False，不会执行除法，不会报错
```

- **链式比较**：Python支持数学风格的链式比较
	- `if 1 < x <= y < 10:`
- **条件表达式**（三目运算符），值在前面，条件在中间
	- `max_num = a if a > b else b`
- 海象运算符 ` := ` 表达式中赋值
	- `if (n := len(data)) > 10:`

## 控制流

- foreach风格的循环
- for-else / while-else
	- `else`块在循环**正常结束**时执行，被`break`中断则不执行
- **pass语句** 空操作占位符，类似于Java中的空大括号`{}`
- match-case：结构化模式匹配
```python
match point: 
	case (x, y) if x == y: 
		print("对角线") 
	case (x, 0): 
		print("其他")
```

**注意：** 不要在遍历时修改列表

```python
# 应该遍历副本
for item in items[:]: 
	if condition: 
		items.remove(item)
```

## 输入输出

### 输入

- input_str = input("请输入：") 
	- 返回字符串，需要类型转换

### 输出

print() 函数

```python
# sep：多个值之间的分隔符（默认空格）
print("a", "b", "c")              # a b c
print("a", "b", "c", sep="-")     # a-b-c

# end：输出结尾字符（默认换行 \n）
print("Hello", end=" ")
print("World")                    # Hello World（同一行）

print("第一行", end="")
print("第二行")                    # 第一行第二行（无换行）
```


1. % 占位符:

```python
name = "Alice"
age = 25
print("姓名: %s, 年龄: %d" % (name, age))
# 常用：%s(字符串) %d(整数) %f(浮点数)
```

2. 字符串.format() 方法
```python
# 不设置顺序
print("int1 = {}, float1 = {}".format(100, 3.14))

# 设置顺序（可重复使用）
print("int1 = {0}, float1 = {1}, 再次 = {0}".format(100, 3.14))

# 命名参数
print("姓名 = {name}, 年龄 = {age}".format(name="Alice", age=25))
```

3. f-string :  f"{表达式:格式说明}" print(f"{x:.2f}")
      - {} 中可以是任意表达式 (` := `需要添加())
      - 加上` = `会打印变量名 ex: `float1 = 1.2`

```python
name = "Alice"
age = 25

# 基本用法
print(f"姓名: {name}, 年龄: {age}")

# 表达式计算
print(f"明年: {age + 1}岁")

# 调试模式：加 = 显示变量名
print(f"{name = }, {age = }")  # name = 'Alice', age = 25

# 海象运算符需要加括号
print(f"{(n := len(name))}")

# 格式化说明
x = 3.14159
print(f"{x:.2f}")  # 3.14
```


性能说明：f-string > .format() > % 格式化
- f-string 编译期处理，字节码已展开，不是运行时解析字符串
- %：运行时调用 PyUnicode_Format()，解析格式串
- .format()：运行时解析

#### 数字格式化

```python
# *****31,415.90******
str2 = "{:*^20,.2f}".format(float1) 
```

使用 ` : ` 来进行转化
- `*` 以` * `填补空白 不写则默认以空格填补
- `< ^ > `：左对齐 居中 右对齐
- 20 ：宽度为20
- `, / _` ：可选 ` , 和 _ `，作为填充
- `.2f `: 小数点保留两位
- `{}` 需要用 `{}` 转译 : 

```python
# hello 对应的位置是 {0}
print ("{} 对应的位置是 {{0}}".format("hello")) 
```

# 容器数据类型

| 容器类型      | 可变性 | 有序性          | 允许重复 | 主要使用场景          | 访问   | 查找   |
| --------- | --- | ------------ | ---- | --------------- | ---- | ---- |
| **list**  | 可变  | 有序           | 是    | 需要有序集合、频繁修改     | O(1) | O(n) |
| **tuple** | 不可变 | 有序           | 是    | 只读数据、字典键、函数返回多值 | O(1) | O(n) |
| **set**   | 可变  | 无序           | 否    | 去重、成员测试、集合运算    | -    | O(1) |
| **dict**  | 可变  | 有序，字典保证按插入顺序 | 键唯一  | 键值对映射、快速查找      | O(1) | O(1) |
| **str**   | 不可变 | 有序           | 是    | 文本处理            | O(1) | O(n) |
## List列表

- 可变有序序列，支持负索引和切片
- 动态数组实现，索引访问O(1)，查找O(n)
- 索引从起始从0开始向后逐个递增，并且从末尾从-1开始逐个向前递减。
- Python列表中元素可以是不同的类型

常见操作：

```python
# 访问和切片
lst[-1]              # 最后一个元素
lst[:]               # 复制整个列表
lst[::-1]            # 反转列表
lst[i:j]             # 子列表
lst[2:4] = ["a", "b", "c"] # 切片修改
lst[1:4] = []        # 删除切片

# 列表推导式
[x for x in range(5)]
[x for x in range(10) if x % 2 == 0]
[x for x in list1]
[(i, j) for i in list1 for j in list2]
print(tuple_list)

# 增加和删除
lst.append(x)        # 末尾添加一个元素 O(1)
lst.extend([5, 6])   # 批量添加，O(k)
lst += [7, 8]        # 等价于extend，O(k)
lst.insert(0, x)     # 指定位置插入 O(n)
lst.pop()            # 删除末尾 O(1)
lst.pop(0)           # 指定位置删除 O(n)
lst.remove(2)        #删除第一个匹配的值,失败抛ValueError
del lst[0]           # 删除索引0，O(n)

# 反转和排序
lst.sort()                   # 原地排序，O(n log n)
lst.sort(reverse=True, key=lambda x: x%10) # 排序个位，降序
sorted(lst)          # 返回新列表
lst.reverse()                # 原地反转，O(n)

# 查找
lst.index(x)         # 第一个x的索引，O(n)
lst.index(x, 2)      # 从索引2开始查找
lst.count(x)         # x出现次数，O(n)
x in lst # 成员判断 O(n) max(lst), min(lst), sum(lst)
```

注意：

```python
# 初始化二维数组
matrix = [[0] * n] * m           # ❌ m行共享同一个列表
matrix = [[0] * n for _ in range(m)]  # ✅ 正确

# 遍历时修改需要使用副本
for item in lst[:]:  # ✅ 遍历副本
    if condition:
        lst.remove(item)
# 或用列表推导式
lst = [x for x in lst if x % 2 != 0]
```

## Tuple(元组)

- 不可变有序序列，比list更轻量
- 可哈希的时候可作为字典键（hashable）

```python
# 创建
t = (1, 2, 3)
t = 1, 2, 3          # 括号可省略
t = (1,)             # 单元素需要逗号

# 元组生成器
tuple_generator = (x for x in range(10))  # 获取生成器对象
tuple1 = tuple(tuple_generator)  # 转换为元组

# 访问（与list相同） 
t[0] # 第一个元素 
t[-1] # 最后一个元素 
t[1:3] # 切片

# 解包
a, b, c = (1, 2, 3)
a, *rest = (1, 2, 3, 4)  # a=1, rest=[2,3,4]
a, *_ = (1, 2, 3) # a=1，忽略其余 
*_, b = (1, 2, 3) # b=3，忽略前面

# 函数返回多值
def func():
    return 1, 2, 3   # 返回元组
x, y, z = func()     # 解包接收
```


## Set

- 无序不重复元素集合
- 基于哈希表实现，查找/插入/删除都是O(1)

```python
# 创建
s = {1, 2, 3}
s = set([1, 2, 2, 3])    # {1, 2, 3}，自动去重
s = set()                # 空集合（不能用{}，那是空字典）
s = set("hello")         # {'h', 'e', 'l', 'o'}

# 添加和删除
s.add(4)                 # 添加元素 O(1)
s.remove(2)              # 删除元素 O(1)，不存在抛KeyError
s.discard(2)             # 删除元素 O(1)，不存在不报错
s.pop()                  # 随机删除并返回一个元素
s.clear()                # 清空集合
s.update()               # 必须是可迭代对象

# 查找
x in s                   # 成员判断 O(1) 
x not in s               # O(1)

# 集合运算（核心功能）
s1 = {1, 2, 3}
s2 = {2, 3, 4}

s1 & s2                  # {2, 3}，交集
s1.intersection(s2)      # 等价于 &

s1 | s2                  # {1, 2, 3, 4}，并集
s1.union(s2)             # 等价于 |

s1 - s2                  # {1}，差集（s1有但s2没有）
s1.difference(s2)        # 等价于 -

s1 ^ s2                  # {1, 4}，对称差（只在一个集合中）
s1.symmetric_difference(s2)  # 等价于 ^

# 子集和超集
s1 <= s2                 # s1是s2的子集
s1.issubset(s2)

s1 >= s2                 # s1是s2的超集
s1.issuperset(s2)

s1.isdisjoint(s2)        # 是否无交集

# 原地修改版本
s1.update(s2)            # s1 |= s2
s1.intersection_update(s2)  # s1 &= s2
s1.difference_update(s2)    # s1 -= s2

# 其他操作
len(s)                   # 长度
max(s), min(s)           # 最大/最小值

# 集合推导式 
s = {x for x in range(10) if x % 2 == 0} 
s = {x**2 for x in [1, 2, 2, 3]} # {1, 4, 9}，自动去重
```

注意：
```python
# set() 接收可迭代对象，会遍历它
set("hello") # 遍历字符串 → 'h', 'e', 'l', 'l', 'o' 
set([1, 2, 3]) # 遍历列表 → 1, 2, 3 
set(["hello"]) # 遍历列表 → "hello"（一个元素）

# 或者直接创建
{"hello"}
```

## Dict
- 键值对映射，Python 3.7+ 保证按插入顺序
- 基于哈希表实现，查找/插入/删除都是O(1)

**常见操作**：
```python
# 创建
d = {'a': 1, 'b': 2}
d = dict(a=1, b=2)           # 关键字参数，语法糖，少打几个引号
d = dict([('a', 1), ('b', 2)])  # 键值对序列
d = dict(zip(keys, values))  # 两个列表合成
d = {}                       # 空字典

# 访问
d['a']                       # 存在返回值，不存在KeyError
d.get('a')                   # 存在返回值，不存在返回None
d.get('c', 0)                # 不存在返回默认值0

# 添加和修改
d['c'] = 3                   # 键不存在则添加，存在则修改
d.setdefault('d', 4)         # 键不存在则设置为4并返回，存在则返回原值
d.update({'e': 5, 'f': 6})   # 批量更新
d.update(g=7, h=8)           # 关键字参数方式

# 删除
d.pop('a')                   # 删除并返回值，不存在抛KeyError
d.pop('z', None)             # 删除并返回值，不存在返回None
del d['b']                   # 删除键值对，不存在KeyError
d.clear()                    # 清空字典

# 视图对象（动态反映字典变化，修改会变化）
keys = d.keys()              # 键视图
values = d.values()          # 值视图
items = d.items()            # 键值对视图

# 复制
d2 = d.copy()                # 浅拷贝

# 合并（Python 3.9+）
d1 | d2                      # 合并，d2覆盖d1
d1 |= d2                     # 原地合并
```


## `*` 解包

序列使用 `*` 字典使用 `**`

### 解包把容器拆成一个个值

#### 函数调用时的展开

- 序列 → 位置参数：`*`

```python
def f(a, b, c):
    print(a, b, c)

args = [1, 2, 3]
f(*args)           # 等价于 f(1, 2, 3)
```

- 字典 → 关键字参数：`**`

```python
def f(a, b, c):
    print(a, b, c)

kwargs = {'a': 1, 'b': 2, 'c': 3}
f(**kwargs)        # 等价于 f(a=1, b=2, c=3)
```

#### 字面量中的展开（合并 / 插入）

- 列表等序列字面量：`*`

```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = [*list1, *list2]  # [1, 2, 3, 4, 5, 6]
```

- 字典字面量：`**`

```python
d1 = {'a': 1, 'b': 2}
d2 = {'c': 3, 'd': 4}
combined = {**d1, **d2}            # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

#### 作为“参数序列”的展开（典型场景）

- `zip` / 解压：`*`
    
```python
matrix = [[1, 2, 3], [4, 5, 6]]
transposed = list(zip(*matrix))

pairs = [(1, 'a'), (2, 'b')]
nums, chars = zip(*pairs)
```

---

### 收集（打包）：把多个值放进一个容器

#### 解包赋值中的收集：`*`

```python
first, *rest = [1, 2, 3, 4]   # rest -> [2, 3, 4]
*front, last = [1, 2, 3, 4]   # front -> [1, 2, 3]
a, *middle, b = [1, 2, 3, 4]  # middle -> [2, 3]
first, *_, last = [1, 2, 3, 4]
```

#### 函数定义中的收集

- 收集位置参数：`*args`
    
```python
def f(*args):
    print(args)    # args 是一个元组

def f(a, b, *rest):
    print(a, b, rest)   # rest 是元组
```

- 收集关键字参数：`**kwargs`
    
```python
def f(**kwargs):
    print(kwargs)  # kwargs 是一个字典

f(a=1, b=2)        # {'a': 1, 'b': 2}
```

## 迭代工具

### enumerate() - 同时获取索引和值

在遍历时同时需要索引和元素值

```python
items = ['a', 'b', 'c']
# 使用 enumerate
for i, item in enumerate(items):
    print(i, item)

# 指定起始索引的初始值，但还是从最开始迭代
for i, item in enumerate(items, start=2):
    print(i, item)
# 2 a
# 3 b
# 4 c

# 常见用法
# 1. 需要索引判断
for i, val in enumerate(nums):
    if i > 0 and val > nums[i-1]:
        print("递增")

# 2. 查找索引
for i, val in enumerate(nums):
    if val == target:
        return i
```

---

### zip() - 并行遍历多个序列

同时遍历多个序列，将对应位置的元素组合成元组

```python
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]
cities = ['NYC', 'LA', 'SF']

# 同时遍历三个列表
for name, age, city in zip(names, ages, cities):
    print(f"{name}, {age}岁, {city}")

# 长度不一致时，以最短的为准
list1 = [1, 2, 3, 4]
list2 = ['a', 'b']
for x, y in zip(list1, list2):
    print(x, y)
# 只输出两对：(1, 'a'), (2, 'b')

# 常见用法
# 1. 创建字典
keys = ['a', 'b', 'c']
values = [1, 2, 3]
d = dict(zip(keys, values))  # {'a': 1, 'b': 2, 'c': 3}

# 2. 转置矩阵（配合 * 解包）
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]
transposed = list(zip(*matrix))  # [(1, 4), (2, 5), (3, 6)]

# 3. 解压（zip的逆操作）
pairs = [(1, 'a'), (2, 'b'), (3, 'c')]
numbers, letters = zip(*pairs)
# numbers = (1, 2, 3)
# letters = ('a', 'b', 'c')
```

# 函数

## 名字绑定 + 对象可变性

调用函数时，**实参对象的引用**被绑定到形参这个名字上。也就是说，函数里面操作的，是“和外面那个名字指向同一个对象”的另一个名字。

关键区别在于：**这个对象是不是可变的（mutable）**。
- 不可变对象 只能通过“返回 + 重新赋值”
	- `int` / `str` / `tuple` 等不可变类型，不能原地改，只能创建新对象：
- 可变对象：共享同一个“盒子”，改就地内容
	- 对于 `list` / `dict` / `set` 等可变对象，如果多个名字指向同一个对象，在函数中做原地修改，外部会看到变化
- `var1 *= 2` vs `var1 = var1 * 2` 只在**可变对象**上有本质区别：对可变对象（例如 `list`）：
	- `a *= 2`：优先调用 `__imul__`，通常是**就地修改**，不更换对象；
	- `a = a * 2`：调用 `__mul__`，**新建一个对象**再绑定到 `a`。

## 参数系统

### 位置参数 / 关键字参数

```python
def print_info(name: str, age: int) -> None:
    print(f"name={name}, age={age}")

# 位置参数：按声明顺序匹配
print_info("zhangsan", 18)

# 关键字参数：按名字匹配，顺序可以打乱
print_info(name="zhangsan", age=18)
print_info(age=18, name="zhangsan")
```

### 默认值参数，带默认值的形参可以省略实参：

```python
def connect(host: str = "localhost", port: int = 3306) -> None:
    ...
```

### 不定长参数：

`*args` 与 `**kwargs` 
- 调用时会先按**位置参数**匹配到前面的形参，
- 多出来的位置参数被打包进 `*args`，
- 多出来的关键字参数被打包进 `**kwargs`。

```python
def func(num1, *var_tuple):
    print(num1, var_tuple)

func(1, 2, 3)   # num1=1, var_tuple=(2, 3)
func(1)         # num1=1, var_tuple=()
```

- 如果不定长的参数后面还有参数，必须通过关键字参数传参
- 如果没有给不定长的参数传参,那么得到的是**空元组**

```python
def print_info1(num1, *var_tuple, num):
    print(num1, var_tuple, num)

print_info1(1, 2, 3, 4, num=5)  # 1 (2, 3, 4) 5
# print_info1(1, 2, 3, 4, 5)    # TypeError
```

`**kwargs` 会把多出来的关键字参数打包成一个字典；
在形参列表中，一旦出现 `**kwargs`，后面就不能再定义其他形参。

```python
def func(**kwargs):
    print(kwargs)

func(a=1, b=2)  # {'a': 1, 'b': 2}
```

### 调用约束：

- 位置-only (`/`) 和 关键字-only (`*`) 参数
	- `/` 前面的 `a, b`：**只能用位置传参**：
	- `*` 后面的 `e, f_`：**必须用关键字传参**：
	- `def f(a, b, /, c, d, *, e, f):`

## 返回值

多个返回值其实是一个元组

```python
def f(a, b, c):
    return a, b, c, [a, b, c]

print(f(1, 2, 3))  # (1, 2, 3, [1, 2, 3])
```

本质上：

- `return a, b, c, [a, b, c]` 是 `return (a, b, c, [a, b, c])` 的语法糖；
- 所谓“返回多个值”，实际就是返回一个元组。

## 作用域：LEGB、闭包，以及 `global` / `nonlocal`

### LEGB：名字查找规则

Python 解释器查一个名字时，按下面顺序查找：
- L（Local）：当前函数的局部作用域；
- E（Enclosing）：外层嵌套函数的局部作用域（闭包）；
- G（Global）：当前模块的顶层作用域；
- B（Built-in）：内建作用域（`len`、`int` 等）。

```python
a = int(2.9)  # Built-in: int
b = 0         # Global

def outer():
    c = 1     # Enclosing
    def inner():
        d = 2  # Local
        print(d, c, b, a)
    return inner

f = outer()
f()  # 2 1 0 2

```

### `global`：修改模块级绑定

`global` 关键字让你在函数内部**声明某个名字指向全局作用域**，从而可以重新绑定它：

```python
var1 = 100  # 全局变量

def function_a():
    global var1
    var1 = 200
    print("var1:", var1)

print(var1)   # 100
function_a()  # var1: 200
print(var1)   # 200
```

### `nonlocal`：修改外层函数的局部绑定

`nonlocal` 允许你修改 **外层函数的局部变量**（即 E 层），典型用在闭包里维护状态：

```python
def function_outer():
    var1 = 1
    print(var1)

    def function_inner():
        nonlocal var1
        var1 = 200

    function_inner()
    print(var1)

function_outer()  # 输出 1 然后 200

```

## 避免误改实参

浅拷贝（`copy.copy` / 切片等）只复制**最外层容器**，内部的元素对象仍然是原来的那些引用。
对于“扁平容器”（例如 `list[int]`、`list[str]`）以及只在最外层“换元素”（`nums[i] = ...`）的场景，浅拷贝已经足够：容器本身是新的，原始容器不再被修改。

```python
# 示例 1：扁平 list，浅拷贝就足够
import copy

def multiply2(nums: list[int]) -> None:
    for i, v in enumerate(nums):
        nums[i] = v * 2  # 只是在最外层 list 槽位上重新绑定新 int

list1 = [1, 2, 3]
list2 = copy.copy(list1)   # 或 list1[:] / list(list1)

multiply2(list2)

print(list1)  # [1, 2, 3]  原始列表未被修改
print(list2)  # [2, 4, 6]  只影响拷贝
```


深拷贝（`copy.deepcopy`）会递归复制整个对象图，包括嵌套的可变对象。

当数据结构中包含嵌套的 list/dict，且操作会对这些**内部子对象**做就地修改（如 `row.append(...)`），浅拷贝无法隔离副作用，此时需要深拷贝或自定义复制逻辑。

```python
# 示例 2：嵌套 list，需要深拷贝才能完全隔离
import copy

def append_x(matrix: list[list[int]]) -> None:
    for row in matrix:
        row.append('x')    # 就地修改内部子列表对象

m1 = [[1, 2], [3, 4]]

m2 = copy.copy(m1)         # 只复制最外层 list
append_x(m2)
print(m1)  # [[1, 2, 'x'], [3, 4, 'x']]  被连带修改

m3 = copy.deepcopy(m1)     # 递归复制所有层级
append_x(m3)
print(m1)  # [[1, 2, 'x'], [3, 4, 'x']]  维持不变
print(m3)  # [[1, 2, 'x', 'x'], [3, 4, 'x', 'x']]
```


## 匿名函数

`lambda` 是匿名函数的语法糖：
- 只能写一个表达式，不能写多条语句；
- 底层仍然是普通函数对象，有自己的命名空间；

```python
# lambda 参数列表: 表达式
from typing import Callable

def opt(a: int, b: int, op: Callable[[int, int], int]) -> int:
    return op(a, b)

def opSum(a,b):
	return a + b

print(opt(1, 2, lambda x, y: x + y))  # 3
print(opt(1,2,opSum))
```
