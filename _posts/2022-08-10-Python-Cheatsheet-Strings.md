---
title: "Python Cheatsheet - Strings"
author: zacgra
layout: post
categories: Code
tags: [python, cheatsheet]
---

## Contents

- [String Indexing](#string-indexing)
- [String Concatenation](#string-concatenation)
- [String Formatting](#string-formatting)
- [String Functions](#string-functions)
- [String Methods](#string-methods)

### String Indexing

```py
    print("Spaghetti"[0])    # => S
    print("Spaghetti"[4])    # => h

    # negative indexing
    print("Spaghetti"[-1])    # => i
    print("Spaghetti"[-4])    # => e

    # ranges
    print("Spaghetti"[1:4])  # => pag
    print("Spaghetti"[4:-1])    # => hett
    print("Spaghetti"[4:4])  # => (empty string)

    # partial ranges
    print("Spaghetti"[:4])  # => Spag
    print("Spaghetti"[:-1])    # => Spaghett

    # single index outside range
    print("Spaghetti"[15])    # => IndexError: string index out of range
    print("Spaghetti"[-15])    # => IndexError: string index out of range

    # index range outside of range
    print("Spaghetti"[:15])    # => Spaghetti
    print("Spaghetti"[15:])    # => (empty string)
    print("Spaghetti"[-15:])    # => Spaghetti
    print("Spaghetti"[:-15])    # => (empty string)
    print("Spaghetti"[15:20])    # => (empty string)
```

### String Concatenation

```py
    # stitch strings together
    print("gold" + "fish")    # => goldfish

    # repeat string given number of times
    print("s"*5)              # => sssss
```

### String Formatting

```py
    first_name = "Billy"
    last_name = "Bob"
    print('Your name is {0} {1}'.format(first_name, last_name))  # => Your name is Billy Bob
```

### String Functions

```py
    # .index
    print("Spaghetti".index("h"))    # => 4
    print("Spaghetti".index("s"))    # => ValueError: substring not found

    # .count
    print("Spaghetti".count("t"))    # => 2
    print("Spaghetti".count("s"))    # => 0

```

### String Methods

```py
    "Hello".upper() # => "HELLO"
    "Hello".lower() # => "hello

    "hello".islower() # => True
    "Hello".isupper() # => False
    "HELLO".isupper() # => True

    "Hello".startswith("He") # => True
    "Hello".endswith("lo") # => True

    "Hello World".split() # => ["Hello", "World"]
    "i-am-a-dog".split("-") # => ["i", "am", "a", "dog"]

    "--".join(["Hello", "World"]) # => "Hello--World"

    "abc".isalpha() # => True
    "abc!@#".isalnum() # => False
    "12345".isdecimal() # => True
    "  ".isspace() # => True
    "This Is A Title".istitle() # => True
    "This Is a Title".istitle() # => False
```
