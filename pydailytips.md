<script type="text/javascript" src="http://159.226.50.212/share/share/shl/scripts/shCore.js"></script>
<script type="text/javascript" src="http://159.226.50.212/share/share/shl/scripts/shBrushPython.js"></script>
<link href="http://159.226.50.212/share/share/shl/styles/shCore.css" rel="stylesheet" type="text/css" />
<link href="http://159.226.50.212/share/share/shl/styles/shThemeDefault.css" rel="stylesheet" type="text/css" />

#Python日常小技巧

by [熊崽Kevin](http://blog.csdn.net/dysj4099)

###1. 处理好判断条件中的True与False

在Python程序中，条件判断是最常见的代码片段了，而对于条件，有一些值得注意的地方。下面是布尔环境中常见的Ture与False情况：

True       				| False
-----------				| -------------
True					| False
-						| None
非空字符串   				| 空字符串 
非0整数     				| 整数0
非空容器 len(x) > 0		| 空容器 len(x) == 0
0, '', [], (), {}, None	| 其他情况


###2. 善用`in`
a) 利用`in`来检查容器内的元素。`in`关键词可以用在`list`、`dict`、`set`、`string`等实现了`__contains__`内建函数的类中。

示例：
<pre class="brush: python">
name = 'Name'

if 'N' in name:
    print 'N in %s' %name
</pre>

b) 尽量用`in`来遍历容器各元素。`in`关键词可以用在`list`、`dict`、`set`、`string`等实现了`__iter__`内建函数的类中。

<pre class="brush: python">
name = ['Jack', 'Paul', 'Tom']

for item in name:
    print item
</pre>

###3. 直接交换变量的值，而不使用中间变量

<pre class="brush: python">
a, b = 10, 20
print 'Before a: %s, b: %s' %(a, b)	# 10, 20
a, b = b, a
print 'After a: %s, b: %s' %(a, b)	# 20, 10
</pre>

###4. 利用`try`来处理容器中的元素访问

在Python中，使用异常检查机制的代价不大（相对Java等其他语言而言），因此不需预先检查容器中对应的key是否存在。

<pre class="brush: python">
a = {'key': 5}

try:
    value = a['key']
except (KeyError, TypeError, ValueError):
    value = None
</pre>

###5. 利用`enumerate`函数同时获取列表的索引及值
<pre class="brush: python">
a = ['a', 'b', 'c']

for i, name in enumerate(a):
    print (i, name)
</pre>

在cookbook里介绍，如果你要计算文件的行数，可以这样写：
<pre class="brush: python">
count = len(open(thefilepath,‘rU’).readlines())
</pre>
前面这种方法简单，但是可能比较慢，当文件比较大时甚至不能工作，下面这种循环读取的方法更合适些。					
<pre class="brush: python">
Count = -1 
For count,line in enumerate(open(thefilepath,‘rU’))： 
    Pass
Count += 1
</pre>

###6. 利用列表映射、解析、过滤机制来创建列表

a) 映射
<pre class="brush: python">
li = [1, 2, 3, 4]
b = [e**2 for e in li] #[1, 4, 9, 16]
</pre>

b) 解析
<pre class="brush: python">
dic = {'a':1, 'b':2, 'c':3, 'd':4}
b = ['%s=%s' %(k, v) for k, v in dic.items()]
 #['a=1', 'c=3', 'b=2', 'd=4']
</pre>

c) 过滤
<pre class="brush: python">
data = [7, 20, 3, 15, 11]
result = [i*3 for i in data if i > 10]
print result
</pre>

###7. Create dict from keys and values using `zip`

<pre class="brush: python">
key = ['a', 'b', 'c', 'd', 'e']
data = [7, 20, 3, 15, 11]
d = dict(zip(key, data))
print d #{'a': 7, 'c': 3, 'b': 20, 'e': 11, 'd': 15}
</pre>

注：如果key与data的长度不同，则会按照最短长度进行匹配

###8. Python中的`range`与`xrange`

a) for i in range(NUM)会返回列表

b) for i in xrange(NUM)会返回迭代器而不是列表

如果不需要返回列表，则最好使用`xrange`，性能很好。

测试代码：
<pre class="brush: python">
import time, datetime

def test_range(num):
    for i in range(num):
        pass

def test_xrange(num):
    for i in xrange(num):
        pass

if __name__=='__main__':
    num = 100000000
    start = time.time()
    test_range(num)
    print 'cost of range: %s' %(time.time() - start)
    start = time.time()
    test_xrange(num)
    print 'cost of xrange: %s' %(time.time() - start)
</pre>

###9. Python中的`lambda`, `filter`, `map`, `reduce`内置函数

a) `lambda`匿名函数

用`lambda`关键字定义一个匿名函数a，对于输入参数x，执行:后的操作并返回。
<pre class="brush: python">
a = lambda x : x + 2
print a(2) #4
</pre>

b) filter(func, seq)

`filter`关键词用func过滤seq中得每个成员，并将func返回为True的成员组成一个新seq返回
<pre class="brush: python">
a = lambda x : x > 2

def a2(x):
    return x <= 2

data = [15, -7, 2, -5, 3, 10]

print filter(a, data)   #[15, 3, 10]
print filter(a2, data)  #[-7, 2, -5]
</pre>

c) map(func, seq)

`map`关键词用func处理seq中的每个成员，并组成一个新的seq返回
<pre class="brush: python">
data = [15, -7, 2, -5, 3, 10]

a3 = lambda x : -x #求反

print map(a3, data)     #[-15, 7, -2, 5, -3, -10]
</pre>

d) reduce(func, seq, init_v)

`reduce`用于迭代计算，func函数必须有两个参数，首先从seq中两个元素取出分别作为func的两个参数(若指定init_v，则取seq的首个元素与init_v作为参数)，返回结果与seq中后续元素迭代计算。

<pre class="brush: python">
def a(x, y):
    return x + y

data = [1, 2, 3, 4, 5, 6, 7, 8, 9]

print reduce(a, data, 0) #45    
print reduce(a, data, 1) #46
</pre>

###10. 字符串连接尽量用`join`而不是`+`

由Python内部实现机制决定了字符串String的底层实现是不可变长度的。

a) 当使用`+`操作符进行字符串连接时:
<pre class="brush: python">
a = "hello "
b = "world"
c = a + b
</pre>
python底层会在内存中额外申请一块内存空间tmp(长度为a,b之和)，并将a,b的内容依次拷贝到tmp中，并用c指向tmp完成字符串连接。如果我们有如下的代码:

<pre class="brush: python">
l_word = []
l_word.append('hello')
...
print l_word #1000

content = ''
for item in l_word:
    content = content + item
</pre>
这样在每次通过`+`做字符串连接的时候，都有一次申请内存空间，并拷贝两个字符串的操作，这样的效率十分低下。

b) 当使用`join`操作符：
继续看刚才的例子
<pre class="brush: python">
l_word = []
l_word.append('hello')
...
print l_word #1000

content = ''.join(l_word)
</pre>
python底层会首先计算`join`操作会涉及到多少元素，总共需要多大的空间，并申请此空间，随后将l_word中每个元素依次拷贝至新申请的空间中。这样，就只会有一次内存申请操作，下面是测试结果，测试的每个字符串平均长度为18，测试了分别连接10, 50, 100, 500, 1000个字符串所用时间(s)

 n     	| 10		| 50		| 100		| 500      | 1000
-------	| --------	| --------	| --------	| -------- | -------
`+`		| .000041	| .000076	| .000115	| .000610  | .00170
`join`	| .000017	| .000069	| .000022	| .000099  | .00016

Good luck :)

<script type="text/javascript">
     SyntaxHighlighter.all()
</script>