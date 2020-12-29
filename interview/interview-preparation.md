# interview-preparation





#### Python程序优化

- 源码
  - 内存优化:单例...
  - 涉及大数据量计算可以引用C语言拓展库
- 数据库
  - 索引
  - 查询
  - 数据库设计
- 缓存
  - redis
  - 内存缓存
- 架构
  - 横向拓展
  - 服务纵向拆分(用户,业务,结算,定时任务)
  - 服务横向拆分(用户足够多时Sharding)
- 语言
  - python 先编译,python -m compileall /root/src
  - CPython改用pypy
  - 改用编译型语言java
  - 使用语言新特性协程



#### Future对象

- 3.2+ 协程之前的future(concurrent.futures)

  > 将可调用对象封装为异步执行 [`Future`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future) 实例由 [`Executor.submit()`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor.submit) 创建。	

  - 异步执行可以由 [`ThreadPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) 使用线程或由 [`ProcessPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor) 使用单独的进程来实现。 两者都是实现抽像类 [`Executor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor) 定义的接口。

  - `submit`(*fn*, */*, **args*, **\*kwargs*)

    调度可调用对象 *fn*，以 `fn(*args **kwargs)` 方式执行并返回 [`Future`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future) 对象代表可调用对象的执行。:`with ThreadPoolExecutor(max_workers=1) as executor:     future = executor.submit(pow, 323, 1235)     print(future.result()) `

  - `map`(*func*, **iterables*, *timeout=None*, *chunksize=1*)

    类似于 [`map(func, *iterables)`](https://docs.python.org/zh-cn/3/library/functions.html#map) 函数，除了以下两点：*iterables* 是立即执行而不是延迟执行的；*func* 是异步执行的，对 *func* 的多个调用可以并发执行。如果 [`__next__()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#iterator.__next__) 已被调用且返回的结果在对 [`Executor.map()`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor.map) 的原始调用经过 *timeout* 秒后还不可用，则已返回的迭代器将引发 [`concurrent.futures.TimeoutError`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.TimeoutError)。 *timeout* 可以为 int 或 float 类型。 如果 *timeout* 未指定或为 `None`，则不限制等待时间。如果 *func* 调用引发一个异常，当从迭代器中取回它的值时这个异常将被引发。使用 [`ProcessPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor) 时，这个方法会将 *iterables* 分割任务块并作为独立的任务并提交到执行池中。这些块的大概数量可以由 *chunksize* 指定正整数设置。 对很长的迭代器来说，使用大的 *chunksize* 值比默认值 1 能显著地提高性能。 *chunksize* 对 [`ThreadPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) 没有效果。在 3.5 版更改: 加入 *chunksize* 参数。

- [`Future`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future) 是一种特殊的 **低层级** 可等待对象，表示一个异步操作的 **最终结果**,线程不安全。

  - `cancelled`()

    如果 Future 已 *取消* 则返回 `True`这个方法通常在设置结果或异常前用来检查 Future 是否已 *取消* 。`if not fut.cancelled():     fut.set_result(42) `

  - `add_done_callback`(*callback*, ***, *context=None*)

    添加一个在 Future *完成* 时运行的回调函数。调用 *callback* 时，Future 对象是它的唯一参数。调用这个方法时 Future 已经 *完成* , 回调函数已被 [`loop.call_soon()`](https://docs.python.org/zh-cn/3.7/library/asyncio-eventloop.html#asyncio.loop.call_soon) 调度。可选键值类的参数 *context* 允许 *callback* 运行在一个指定的自定义 [`contextvars.Context`](https://docs.python.org/zh-cn/3.7/library/contextvars.html#contextvars.Context) 对象中。如果没有提供 *context* ，则使用当前上下文。可以用 [`functools.partial()`](https://docs.python.org/zh-cn/3.7/library/functools.html#functools.partial) 给回调函数传递参数，例如:`# Call 'print("Future:", fut)' when "fut" is done. fut.add_done_callback(     functools.partial(print, "Future:")) `在 3.7 版更改: 加入键值类形参 *context*。请参阅 [**PEP 567**](https://www.python.org/dev/peps/pep-0567) 查看更多细节。

  - `result`()

    返回 Future 的结果。如果 Future 状态为 *完成* ，并由 [`set_result()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.set_result) 方法设置一个结果，则返回这个结果。如果 Future 状态为 *完成* ，并由 [`set_exception()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.set_exception) 方法设置一个异常，那么这个方法会引发异常。如果 Future 已 *取消*，方法会引发一个 [`CancelledError`](https://docs.python.org/zh-cn/3.7/library/asyncio-exceptions.html#asyncio.CancelledError) 异常。如果 Future 的结果还不可用，此方法会引发一个 [`InvalidStateError`](https://docs.python.org/zh-cn/3.7/library/asyncio-exceptions.html#asyncio.InvalidStateError) 异常。

  - `set_result`(*result*)

    将 Future 标记为 *完成* 并设置结果。如果 Future 已经 *完成* 则抛出一个 [`InvalidStateError`](https://docs.python.org/zh-cn/3.7/library/asyncio-exceptions.html#asyncio.InvalidStateError) 错误。

  - `set_exception`(*exception*)

    将 Future 标记为 *完成* 并设置一个异常。如果 Future 已经 *完成* 则抛出一个 [`InvalidStateError`](https://docs.python.org/zh-cn/3.7/library/asyncio-exceptions.html#asyncio.InvalidStateError) 错误。

- **重要**

  该 Future 对象是为了模仿 [`concurrent.futures.Future`](https://docs.python.org/zh-cn/3.7/library/concurrent.futures.html#concurrent.futures.Future) 类。主要差异包含：

  - 与 asyncio 的 Future 不同，[`concurrent.futures.Future`](https://docs.python.org/zh-cn/3.7/library/concurrent.futures.html#concurrent.futures.Future) 实例不是可等待对象。
  - [`asyncio.Future.result()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.result) 和 [`asyncio.Future.exception()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.exception) 不接受 *timeout* 参数。
  - Future 没有 *完成* 时 [`asyncio.Future.result()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.result) 和 [`asyncio.Future.exception()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.exception) 抛出一个 [`InvalidStateError`](https://docs.python.org/zh-cn/3.7/library/asyncio-exceptions.html#asyncio.InvalidStateError) 异常。
  - 使用 [`asyncio.Future.add_done_callback()`](https://docs.python.org/zh-cn/3.7/library/asyncio-future.html#asyncio.Future.add_done_callback) 注册的回调函数不会立即调用，而是被 [`loop.call_soon()`](https://docs.python.org/zh-cn/3.7/library/asyncio-eventloop.html#asyncio.loop.call_soon) 调度。
  - asyncio Future 不能兼容 [`concurrent.futures.wait()`](https://docs.python.org/zh-cn/3.7/library/concurrent.futures.html#concurrent.futures.wait) 和 [`concurrent.futures.as_completed()`](https://docs.python.org/zh-cn/3.7/library/concurrent.futures.html#concurrent.futures.as_completed) 函数。