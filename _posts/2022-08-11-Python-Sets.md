---
title: "Python - Sets"
author: zacgra
layout: post
categories: Code
tags: [python, cheatsheet]
---

## Sets

---

- [Set Constructors](#set-constructors)
- [Empty Sets](#empty-sets)
- [Set Functions](#set-functions)
- [Set Operations](#set-operations)
- [Subsets and Supersets](#subsets-and-supersets)

---

### Set Constructors

- Sets can be made up of strings, numbers, tuples, but elements must be immutable, so no lists or dictionaries as individual elements.

```py
# lists
>>> set(['foo', 'bar', 'baz', 'foo', 'qux'])
{'qux', 'foo', 'bar', 'baz'}

# tuples
>>> set(('foo', 'bar', 'baz', 'foo', 'qux'))
{'qux', 'foo', 'bar', 'baz'}

# strings
>>> set('foo')
{'o', 'f'}

# set literal
>>> {'foo', 'bar', 'baz', 'foo', 'qux'}
{'qux', 'foo', 'bar', 'baz'}

>>> {'q', 'u', 'u', 'x'}
{'x', 'q', 'u'}

# set literal doesn't work on single string
>>> {'foo'}
{'foo'}
```

### Empty Sets

```py
# NOTE: empty curly braces initalize a dict, not a set!
>>> x = {}
>>> type(x)
<class 'dict'>

>>> x = set()
>>> type(x)
<class 'set'>

# empty set is falsy
>>> bool(x)
False
>>> x or 1
1
>>> x and 1
set()
```

### Set Functions

```py
>>> x = {'foo', 'bar', 'baz'}
>>> len(x)
3
>>> 'bar' in x
True
>>> 'qux' in x
False
```

### Set Operations

```py
x1 = {'foo', 'bar', 'baz'}
x2 = {'baz', 'qux', 'quux'}

# Union
#   elements that are x1 or in x2, including items in both
>>> x1 | x2
{'baz', 'quux', 'qux', 'bar', 'foo'}
>>> x1.union(x2)
{'baz', 'quux', 'qux', 'bar', 'foo'}
#note, union converts iteraable as argument, doesn't need to be a set

# Intersection
#   elements that are in both x1 and x2
>>> x1 & x2
{'baz'}
>>> x1.intersection(x2)
{'baz'}

# Difference
#   difference is x1 with x2 elements removed
>>> x1 - x2
{'foo', 'bar'}
>>> x1.difference(x2)
{'foo', 'bar'}

# Symmetric Difference
#   symmetric difference is elements in either x1 or x2, but not in both
>>> x1.symmetric_difference(x2)
{'foo', 'qux', 'quux', 'bar'}
>>> x1 ^ x2
{'foo', 'qux', 'quux', 'bar'}
# only ^ operator allows multiple arguments

# Disjoint
#   returns boolean of whether two sets have no elements in common
>>> x2 - {'baz'}
{'quux', 'qux'}
>>> x1.isdisjoint(x2 - {'baz'})
True
```

### Subsets and Supersets

```py
# Subsets
>>> x1.issubset({'foo', 'bar', 'baz', 'qux', 'quux'})
True
>>> x1 <= x2
False
# a set is a subset of itself
>>> x.issubset(x)
True
>>> x <= x
True

# Proper Subsets
#   A set x1 is considered a proper subset of another set x2 if every element of x1 is in x2, and x1 and x2 are not equal.
>>> x1 = {'foo', 'bar'}
>>> x2 = {'foo', 'bar', 'baz'}
>>> x1 < x2
True

>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'bar', 'baz'}
>>> x1 < x2
False

# Supersets
#   a set x1 is considered a superset of another set x2 if x1 contains every element of x2.
```
