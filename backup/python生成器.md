在 Python 2 时代，如果你想用 range() 生成一个非常大的数字序列——比如 0 到 1 亿间的所有数字，速度会非常慢。这是因为 range()需要组装并返回一个巨大的列表，整个计算与内存分配过程会耗费大量时间。

但到了 Python 3，调用 range(100000000) 瞬间就会返回结果。因为它不再返回列表，而是返回一个类型为 range 的惰性计算对象。
```
>>> r = range(100000000)
>>> r
range(0, 100000000) 
>>> type(r)
<class 'range'>
>>> for i in r: 
...     print(i)
...
0
1
...
```

r 是 range 对象，而非装满数字的列表。

只有在迭代 range 对象时，它才会不断生成新的数字。

range()的进化过程虽然简单，但它其实代表了一种重要的编程思维——按需生成，而不是一次性返回。

在日常编码中，实践这种思维可以有效提升代码的执行效率。Python 里的生成器对象非常适合用来实现“按需生成”。

一个最简单的生成器如下：

```
def generate_even(max_number):
    """一个简单生成器，返回 0 到 max_number 之间的所有偶数"""
    for i in range(0, max_number):
        if i % 2 == 0:
            yield i

for i in generate_even(10):
    print(i)
```

执行后输出：
```
0
2
4
6
8
```

虽然都是返回结果，但 yield 和 return的最大不同之处在于，return 的返回是一次性的，使用它会直接中断整个函数执行，而 yield 可以逐步给调用方生成结果：
```
>>> i = generate_even(10)
>>> next(i)
0
>>> next(i) 
2
```

因为生成器是可迭代对象，所以你可以使用 list() 等函数方便地把它转换为各种其他容器类型：
```
>>> list(generate_even(10))
[0, 2, 4, 6, 8]
```

实践中可以用生成器来替代列表。

在日常工作中，我们经常需要编写下面这样的代码：
```
def batch_process(items):
    """
    批量处理多个 items 对象
    """
    # 初始化空结果列表
    results = []
    for item in items:
        # 处理 item，可能需要耗费大量时间……
        # processed_item = ...
        results.append(processed_item)
    # 将拼装后的结果列表返回
    return results
```

这样的函数遵循同一种模式：“初始化结果容器→处理→将结果存入容器→返回容器”。这个模式虽然简单，但它有两个问题。

一个问题是，如果需要处理的对象 items过大，batch_process() 函数就会像 Python 2 里的 range() 函数一样，每次执行都特别慢，存放结果的对象 results 也会占用大量内存。

另一个问题是，如果函数调用方想在某个 processed_item 对象满足特定条件时中断，不再继续处理后面的对象，现在batch_process() 函数也做不到——它每次都得一次性处理完所有 items 才会返回。

为了解决这两个问题，我们可以用生成器函数来改写它。简单来说，就是用 yield item 替代 append 语句：
```
def batch_process(items):
    for item in items:
        # 处理 item，可能需要耗费大量时间……
        # processed_item = ...
        yield processed_item
```

生成器函数不仅看上去更短，而且很好地解决了前面的两个问题。当输入参数 items很大时，batch_process() 不再需要一次性拼装返回一个巨大的结果列表，内存占用更小，执行起来也更快。

如果调用方需要在某些条件下中断处理，也完全可以做到：
```
# 调用方
for processed_item in batch_process(items):
    # 如果某个已处理对象过期了，就中断当前的所有处理
    if processed_item.has_expired():
        break
```

在上面的代码里，当调用方退出循环后，batch_process() 函数也会直接中断，不需要再接着处理 items 里剩下的内容。