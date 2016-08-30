---
title: Fluent-Python-0-Python-Data-Model
tags: Python, Fluent Python
categories: 
keywords:
description:
---


# Special Methods 

Special methods are called by the Python interpreter, not by you !

- `__repr__` are for programmers, for debugging and logging.
- `__str__` are for presentation to  end-users.

If no customm `__str__`, Python will call `__repr__`.

bool(x) will call `x.__bool__()`. If `__bool__` does not implemented, Python will try invoke `x.__len()__`. 

# Sequenece

- Contrainer sequences: `list`, `tuple`, `collections.deque` can hold items of different types.
- Flat sequences: `str`, `bytes`, `bytearray`, `memoryview` and `array.array` hold items of one type.


## Generator expressions

Genexp saves memory because it yields items one by one using the itteration protocol instead of building a whole list.


{% codeblock Genexp lang:python %}
symbols = 'abcde'
tuple(ord(symbol) for symbol in symbols)
{% endcodeblock %}

## Tuples are not just immutable lists

Tuple do doube-duty:

1. Used as immutable lists.
2. Used as records with no field names.

### Tuples as recodes: `collections.namedtuple`

The `collections.namedtuple` function is a factory that produces subclasses of tuple enhanced with field names and a class name-which helps deubgging.

Three usefule methods of objects created from `collections.namedtuple`:

- ` _fiedlds` is a tuple with field names of class.
- `_make(iterable)` instantiate a named tuple from a iterable.
- `_asdict()` returns a `collections.OrderDict` built from the named tuple instance.

### Tuples as immutable lists


# Slicing

# Tips

`__getitem__` supports slicing, iteration, `in` operation



# 参考资料

# 更新日志
