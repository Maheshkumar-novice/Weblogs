---
layout: post
title:  "Fun With Python OS Imports"
date:   2024-05-26 12:41:58 +0530
categories: learning
---

<nav>
  <h4>Table of Contents</h4>
  * this unordered seed list will be replaced by toc as unordered list
  {:toc}
</nav>

# Introduction

In this article we are going to explore about the ways in which we can import and use `os` module in python. This can be applied to other modules based on the context.

## The Standard Way

```python
import os

os.system('ls')
```

```python
from os import system

system('ls')
```

## Using `importlib`

```python
import importlib

importlib.import_module('os').system('ls')
```

## Using `__import__`

```python
os = __import__('os')

os.system('ls')
```

## Using `sys`

```python
# If `sys` and `os` is already imported or accessible.

sys.modules['os'].system('ls')
```

## Using `eval`

```python
os = eval('__import__("os")')

os.system('ls')
```

## Using `exec`

```python
exec('import os; os.system("ls")')
```


## Other Ways

### Using Hex

```python
eval(bytes.fromhex('5f5f696d706f72745f5f28226f732229')).system('ls')

exec(bytes.fromhex('696d706f7274206f733b6f732e73797374656d28226c732229'))

__import__(bytes.fromhex('6f73').decode()).system('ls')
```

### Using String Manipulation

```python
__import__(''.join(['o', 's'])).system('ls')

__import__('o1s1'.replace('1', '')).system('ls')
```

### Using other packages

```python
# If already imported package exposes `os`

logging.os.system('ls')

getattr(logging, 'o1s1'.replace('1', '')).system('ls')
getattr(logging, bytes.fromhex('6f73').decode()).system('ls')
```

```python
getattr(getattr(logging, 'o1s1'.replace('1', '')), 's1y1s1t1e1m1'.replace('1', ''))('ls')
```

```python
import operator

f = operator.methodcaller('o1s1'.replace('1', ''))
os = f(logging)

f = operator.methodcaller('s1y1s1t1e1m1'.replace('1', ''), 'ls')
f(os)
```

```python
import operator

l = globals().get('l1o1g1g1i1n1g1'.replace('1', ''))

o = getattr(l, 'o1s1'.replace('1', ''))

f = operator.methodcaller('s1y1s1t1e1m1'.replace('1', ''), 'ls')
f(o)
```

# Conclusion

We can also combine `hex`, `replace` and other methods in different ways. There are a plenty of ways to use the `os` package even if we restrict it somehow. I'm still exploring the possibilities. If I come to know more possibilities, I'll update here.
