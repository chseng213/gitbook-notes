---
description: 记录项目过程中使用过的魔术方法
---

# 魔术方法

\[TOC\]

## 1`__new__`

> 最先调用并返回类实例
>
> `__new__`是类的静态方法，即使没有被加上静态方法装饰器。

### 1.1使用场景

**创建实例需要满足特定条件**

* 满足特定条件

  > 18岁为成人

```python
class Person:    
    def __new__(cls,age, name):        
        if age and age>18:                      
            inst = super(Person, cls).__new__(cls, age, name)
            return inst
    # __new__ 未返回实例时,__init__,__str__,__repr__等实例方法均不会被调用
    def __init__(self,age,name):
        self.name=name
        self.age=age
```

**单例模式的实现**

```python
class Singleton:
    _instance = None
    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls._instance
```

### 1.2相关方法`__init__`

