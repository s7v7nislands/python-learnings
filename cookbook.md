#python cookbook

###python的介绍

-	动态语言
-	所以都是对象
-	cpython,pypy,jython, ironpython

以下介绍都是以cpython为例子:

####虚拟机

-	stack based
-	bytecode

####内存分配管理

-	引用计数
-	gc

```python
# 尽量不要定义,应该显式的定义close()之类的函数
# python3已经有改进: pep-0442
__del__()
```

```
Starting with version 1.5, Python guarantees that globals whose name begins with a single underscore are deleted from their module before other globals are deleted; if no other references to such globals exist, this may help in assuring that imported modules are still available at the time when the __del__() method is called
```

####namespace

-	LEGB Rule
	-	local
	-	enclosed
	-	global
	-	builtin
-	name binding
-	global, nonlocal

```python
a = 'global'

def outer():

    def len(in_var):
        print('called my len() function: ', end="")
        l = 0
        for i in in_var:
            l += 1
        return l

    a = 'local'

    def inner():
        global len
        print("len func:", len)
        nonlocal a
        a += ' variable'
        print("len: ", len(a))
    inner()
    print('a is', a)  
    print(len(a))

outer()

print(len(a))
print('a is', a)
```

####bytecode

```python
import dis
import inspect
```

####函数参数传递方式

-	引用

```python
i = 5
j = []

def test(i, j):
    i += 1
    j.append(1)

test(i, j)
print(i, j)

#why?
```

-	mutable, immutable

```python
def test(default = []):
    default.append(1)
    print(default)

def test1(default = None):
    if default is None:
        default = []
    default.append(1)
    print(default)

test()
test()

#why?
```

###开发环境

-	python2.7
-	virtualenv
-	pip
-	[pew](https://github.com/berdario/pew)

###代码风格

-	pep8
-	pyflake
-	pylint
-	yapf

###编码问题

-	latin1

```python
# -*- coding: utf-8
#测试代码
a = '你好'
b = u'你好'

#print(a.encode('gbk'))          #报错,用ascii来decode
print(a.decode('utf-8').encode('gbk'))
print(b.encode('gbk'))
print(b)

python t.py > /dev/null
why?
```

###unpack

```python
>>>name, age = "xbjiang", 18
>>>
>>>name, age = ("xbjiang", 18)
>>>
>>>name, age = ("xbjiang", 18, "software engineer")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)
>>>
>>> name, age, job = ("xbjiang", 18)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: need more than 2 values to unpack
```

---

```python
>>> name, *other = ("xbjiang", 18, "software engineer")
>>> name, other
('xbjiang', [18, 'software engineer'])
```

**python3**才支持

---

##丰富的数据结构

-	tuple, list, set, frozenset
-	array
-	heapq
-	Queue
-	itertools
-	collections

	-	namedtuple
	-	deque
	-	defaultdict
	-	OrderedDict
	-	Counter

---

-	deque支持双向链表, 注意list的append是N(1),但是[1:],底层有数组实现
-	deque只有pop,append是**线程安全**
-	Queue是线程安全,底层使用deque

```python
from collections import deque
import Queue
```

defaultdict的实现:

```python
>>> class Counter(dict):
...     def __missing__(self, key):
...         return 0
>>> c = Counter()
>>> c['red']
0
>>> c['red'] += 1
>>> c['red']
1
```

---

###iterate protocol

python3的改进

-	iter(),next(),StopIteration
-	generator
	-	yield
	-	yield from
-	列表解析

###性能分析

-	cProfile
-	timeit

###性能优化

-	c扩展
	-	cffi
	-	ctype
-	cython
-	pypy

##todo list

-	== vs is

	-	id()
	-	cpython实现可能会cache
	-	keyword.iskeyword()
	-	while 1 vs while True

-	slice

	-	\[::-1]

-	with

-	sorted(key=)

-	string.join()

-	re,缓存100

-	reload(): 怎样动态更新

-	requests

访问url要注意encode,requests会自动urlencode

session的使用: 可以实现自动重试

```python
session = requests.session()
adapter = requests.adapters.HTTPAdapter(max_retries=1)

session.mount("http://", adapter)
session.mount("https://", adapter)
```

```python
import requests
from urllib import quote, unquote
from urlparse import parse_qs, parse_qsl
```

-	python -m

```shell
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...

$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 ...


$  python3 -m json.tool
```

---

###python internal

```python
import inspect
import dis
```

---

###python多线程问题

GIL,python3.2有改进 why?

-	GIL
-	影响io性能
-	方便写c扩展

###python资料集合

-	[python doc](https://docs.python.org)
-	[python cookbook](http://chimera.labs.oreilly.com/books/1230000000393)
-	[awesome-python](https://github.com/vinta/awesome-python)
