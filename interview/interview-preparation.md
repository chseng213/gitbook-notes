### Interview preparation



#### Python程序优化

+ 源码
  + 内存优化:单例...
  + 涉及大数据量计算可以引用C语言拓展库
+ 数据库
  + 索引
  + 查询
  + 数据库设计
+ 缓存
  + redis
  + 内存缓存
+ 架构
  + 横向拓展
  + 服务纵向拆分(用户,业务,结算,定时任务)
  + 服务横向拆分(用户足够多时Sharding)
+ 语言
  + python 先编译,python -m compileall /root/src
  + CPython改用pypy
  + 改用编译型语言java
  + 使用语言新特性协程



#### Future对象

+ 3.2+ 协程之前的future(concurrent.futures)

  > 将可调用对象封装为异步执行 [`Future`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future) 实例由 [`Executor.submit()`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor.submit) 创建。	

  + 异步执行可以由 [`ThreadPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) 使用线程或由 [`ProcessPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor) 使用单独的进程来实现。 两者都是实现抽像类 [`Executor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor) 定义的接口。

  + `submit`(*fn*, */*, **args*, **\*kwargs*)

    调度可调用对象 *fn*，以 `fn(*args **kwargs)` 方式执行并返回 [`Future`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future) 对象代表可调用对象的执行。:`with ThreadPoolExecutor(max_workers=1) as executor:     future = executor.submit(pow, 323, 1235)     print(future.result()) `

  - `map`(*func*, **iterables*, *timeout=None*, *chunksize=1*)

    类似于 [`map(func, *iterables)`](https://docs.python.org/zh-cn/3/library/functions.html#map) 函数，除了以下两点：*iterables* 是立即执行而不是延迟执行的；*func* 是异步执行的，对 *func* 的多个调用可以并发执行。如果 [`__next__()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#iterator.__next__) 已被调用且返回的结果在对 [`Executor.map()`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor.map) 的原始调用经过 *timeout* 秒后还不可用，则已返回的迭代器将引发 [`concurrent.futures.TimeoutError`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.TimeoutError)。 *timeout* 可以为 int 或 float 类型。 如果 *timeout* 未指定或为 `None`，则不限制等待时间。如果 *func* 调用引发一个异常，当从迭代器中取回它的值时这个异常将被引发。使用 [`ProcessPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor) 时，这个方法会将 *iterables* 分割任务块并作为独立的任务并提交到执行池中。这些块的大概数量可以由 *chunksize* 指定正整数设置。 对很长的迭代器来说，使用大的 *chunksize* 值比默认值 1 能显著地提高性能。 *chunksize* 对 [`ThreadPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) 没有效果。在 3.5 版更改: 加入 *chunksize* 参数。

+ [`Future`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future) 是一种特殊的 **低层级** 可等待对象，表示一个异步操作的 **最终结果**。