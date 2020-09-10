# 内置方法学习

\[TOC\]

## `vars`

```text
vars([object]) -> dictionary
Without arguments, equivalent to locals().
With an argument, equivalent to object.__dict__.

A TypeError exception is raised if an object is specified but it doesn’t have a __dict__ attribute (for example, if its class defines the __slots__ attribute).
```

> 以键值对的形式返回对象的属性和值
>
> **当参数为类时,返回的是`mappingproxy`:类dict,但是无`__setattr__` 方法**

```text
>>>vars()
{'__builtins__': {'__name__': 'builtins', '__doc__': "Built-in functions, exceptions, and other objects.\n\nNoteworthy: None is the `nil' object; Ellipsis represents `...' in slices.", '__package__': '', '__loader__':.....
```

## `zip`

## `enumerate`

## `fliter`

## `reduce`

## `map`

## `partial`

## `isinstance`

## `lambda`

## `chain`

