---
title: Decorators-and-Closures
tags: Python3, Decorators, Closures
categories: Python3
keywords: Python3, Decorators, Closures
description: Understanding of the decorators and closures.
---

# Decorators 101

Two crucial facts:

1. They have the power to replace the decorated function with a different one.
2. They are executed immediately when a module is loaded.

# Closures

A closures is function that retains the bindings of the free variable that exist when the function is defined, so that they can be used later when the function is invoked and the defining scope is no longer available. (Keep continues information in the function object.)

# Decorators in Standard Library

1. ``functools.lru_cache``: It implements memoization: an optimization technology which works by saving the results of previous invocations of an expensive function, avoiding repeat computations on previously used arguments.
2. ``functools.singledispatch``: Transforms a function into a single-dispatch generic function.


# 参考资料

# 更新日志
