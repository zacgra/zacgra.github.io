---
title: "Python Cheatsheet - Virtual Environments"
author: zacgra
layout: post
categories: Code
tags: [python, cheatsheet]
---

- [Virtual Environments](#virtual-environments)

## Virtual Environments

(https://docs.python.org/3/library/venv.html)

```console
python3 -m venv /path/to/new/virtual/environment
```

> creates target directory containing pyvenv.cfg file with `home` key pointing to Python installation used to run command
> creates bin subdirectory containing copy/symlink of Python binaries
> creates a lib/python3.10.5/site-packages subdirectory
