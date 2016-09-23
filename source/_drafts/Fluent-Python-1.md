---
title: Fluent-Python-Dictionary
tags:
categories:
keywords:
description:
---

# Dict

`dict.setdefault` is much efficent in handling the missing key value.

# Variations of dict

- `collections.OrderDict`: maintains keys in insertion order, allowing iteration over items in a predictable order.
- `collections.ChainMap`: holds a list of mappings which can be searched as one.
- `collections.count`: a mapping that holds an integer count for each key.
- `collections.UserDict`: a pure Python implementation of a mapping that works like a standard dict.


# Subclassing UserDict

Note that UserDict does not inherit from dict, but has an internal dict instance, called data, which holds the actual items.

# 参考资料

# 更新日志
