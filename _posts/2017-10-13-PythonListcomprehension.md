---
layout: post
title: "Python列表推导式"
date: 2017-10-13
categories: [Python]
datetime: 2017-10-13
tags: [Python]

---

* content
{:toc}

列表推导式（list comprehension）是利用其他列表创建新列表（类似于数学术语中的集合推导式）的一种方法。它的工作方式类似于for循环。
<!-- more -->

> 列表生成式（list comprehension）是一种简化代码的优美方法。根据python文档，“列表生成式提供了一种创建列表的简洁方法。

# 1.平方列表
如果你想创建一个包含1到10的平方的列表，你可以这样做：
```python
squares = []
for x in range(10):
    squares.append(x**2)
```
这是一个简单的例子，但是使用列表生成式可以更简洁地创建这个列表。
```
squares = [x**2 for x in range(10)]
```
这个最简单的列表生成式由方括号开始，方括号内部先是一个表达式，其后跟着一个for语句。列表生成式总是返回一个列表。

# 2.整除3的数字列表
```
numbers = []
for x in range(100):
    if x % 3 == 0:
        numbers.append(x)
```
你可以在列表生成式里包含一个if语句，来有条件地为列表添加项。为了创建一个包含0到100间能被3整除的数字列表，可以使用列表推导式：

```
numbers = [x for x in range(100) if(x % 3 == 0)]
```

# 3.找出质数
这通常要使用好几行代码来实现。
```
noprimes = []
for i in range(2, 8):
    for j in range(i*2, 50, i):
        noprimes.append(j)
primes = []
for x in range(2, 50):
    if x not in noprimes:
        primes.append(x)
```
不过，你可以使用两个列表生成式来简化代码。
```
noprimes = [j for i in range(2, 8) for j in range(i*2, 50, i)]
primes = [x for x in range(2, 50) if x not in noprimes]
```
第一行代码在一个列表生成式里使用了多层for循环。第一个循环是外部循环，第二个循环是是内部循环。为了找到质数，我们首先找到一个非质数的列表。通过找出2-7的倍数来产生这个非质数列表。然后我们循环遍历数字并查看每个数字是否在非质数列表。

修正：正如reddit上的shoyer指出的，使用集合（set）来查找noprimes（代码里的属性参数，译者注）效率更高。由于noprimes应该只包含唯一的值，并且我们频繁地去检查一个值是否存在，所以我们应该使用集合。集合的使用语法和列表的使用语法类似，所以我们可以这样使用：
```
noprimes = set(j for i in range(2, 8) for j in range(i*2, 50, i))
primes = [x for x in range(2, 50) if x not in noprimes]
```
# 4.嵌套列表降维
假设你有一个列表的列表（列表里包含列表）或者一个矩阵，并且你想把它降维到一个一维列表。你可以这样做：
```
matrix = [[0,1,2,3], [4,5,6,7], [8,9,10,11]]
flattened = []
for row in matrix:
    for i in row:
        flattened.append(i)
```
使用列表生成式：

```
flattened = [i for row in matrix for i in row]
```
这使用了两个for循环去迭代整个矩阵。外层（第一个）循环按行迭代，内部（第二个）循环对该行的每个项进行迭代。

