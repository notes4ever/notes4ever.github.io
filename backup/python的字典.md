字典的动态解包。

在字典中使用 **dict_obj 表达式，可以动态解包 dict_obj 字典的所有内容。
```
>>> d1 = {'name': 'apple'}
>>> d2 = {'price': 10}

# d1、d2 原始值不会受影响
>>> {**d1, **d2}
{'name': 'apple', 'price': 10}
```

除了使用 ** 解包字典，你还可以使用单星号 * 运算符来解包任何可迭代对象：
```
>>> [1, 2, *range(3)]
[1, 2, 0, 1, 2]

>>> l1 = [1, 2]
>>> l2 = [3, 4]
# 合并两个列表
>>> [*l1, *l2]
[1, 2, 3, 4]
```

合理利用 * 和 ** 运算符，可以帮助我们高效构建列表与字典对象。

Python 发布了 3.9 版本。在这个版本中，字典类型新增了对 |运算符的支持。只要执行 d1 | d2，就能快速拿到两个字典合并后的结果：
```
>>> d1 = {'name': 'apple'}
>>> d2 = {'name': 'orange', 'price': 10}
>>> d1 | d2
{'name': 'orange', 'price': 10}
>>> d2 | d1  ➊
{'name': 'apple', 'price': 10}
```

运算顺序不同，会影响最终的合并结果。d1和d2本身不会被改变。d1.update(d2) 则会改变d1原来的值。

dict.setdefault(key, default) 会产生两种结果：当 key 不存在时，该方法会把 default 值写入字典的 key 位置，并返回该值；假如 key 已经存在，该方法就会直接返回它在字典中的对应值。

假设你只是单纯地想去掉某个键，并不关心它存在与否、删除有没有成功，那么使用 dict.pop(key, default) 方法就够了。

只要在调用 pop 方法时传入默认值 None，在键不存在的情况下也不会产生任何异常。
```
d.pop(key, None)
```

和列表类似，字典同样有自己的字典推导式。（比元组待遇好多啦！）你可以用它来方便地过滤和处理字典成员：
```
>>> d1 = {'foo': 3, 'bar': 4}
>>> {key: value * 10 for key, value in d1.items() if key == 'foo'}
{'foo': 30}
```

字典的有序性与无序性的理解。

在 Python 3.6 版本以前，几乎所有开发者都遵从一条常识：“Python 的字典是无序的。”这里的无序指的是：当你按照某种顺序把内容存进字典后，就永远没法按照原顺序把它取出来了。

但 Python 语言在不断进化。Python 3.6 为字典类型引入了一个改进：优化了底层实现，同样的字典相比 3.5 版本可节约多达 25% 的内存。而这个改进同时带来了一个有趣的副作用：字典变得有序了。一开始，字典变为有序只是作为 3.6 版本的“隐藏特性”存在。但到了 3.7 版本，它已经彻底成了语言规范的一部分。如今当你使用字典时，假如程序的目标运行环境是 Python 3.7 或更高版本，那你完全可以依赖字典类型的这种有序特性。

但如果你使用的 Python 版本没有那么新，也可以从 collections 模块里方便地拿到另一个有序字典对象 OrderedDict，它可以在 Python 3.7 以前的版本里保证字典有序：
```
>>> from collections import OrderedDict
>>> d = OrderedDict()
>>> d['FIRST_KEY'] = 1
>>> d['SECOND_KEY'] = 2

>>> for key in d:
...     print(key)
FIRST_KEY
SECOND_KEY
```

OrderedDict 比起普通字典仍然有一些优势。最直接的一点是，OrderedDict 把“有序”放在了自己的名字里，因此当你在代码中使用它时，其实比普通字典更清晰地表达了“此处会依赖字典的有序特性”这一点。

另外从功能上来说，OrderedDict 与新版本的字典其实也有着一些细微区别。比如，在对比两个内容相同而顺序不同的字典对象时，解释器会返回 True 结果；但如果是 OrderedDict对象，则会返回 False，OrderedDict对象的键的顺序也是比较条件。

除此之外，OrderedDict 还有 .move_to_end() 等普通字典没有的一些方法。所以，即便 Python 3.7 及之后的版本已经提供了内置的“有序字典”，但 OrderedDict 仍然有着自己的一席之地。

内置模块 collections里的 defaultdict 类型。

为了更好地理解 defaultdict 的特点，我们来做个小实验。首先初始化一个空 defaultdict 对象：
```
>>> from collections import defaultdict
>>> int_dict = defaultdict(int)
```

然后直接对一个不存在的 key 执行累加操作。普通字典在执行这个操作时，会抛出 KeyError 异常，但 defaultdict 不会：
```
>>> int_dict['foo'] += 1
```

当 int_dict 发现键 'foo' 不存在时，它会调用 default_factory——也就是 int()——拿到结果 0，将其保存到字典后再执行累加操作：
```
>>> int_dict
defaultdict(<class 'int'>, {'foo': 1})
>>> dict(int_dict)
{'foo': 1}
```

通过引入 defaultdict 类型，代码的两处初始化逻辑都变得更简单了。

如果你想创建一个自定义字典，继承 collections.abc 下的 MutableMapping 抽象类，比继承原生的dict是个更好的选择，
继承原生dict有行为不一致等问题；而对于列表等其他容器类型来说，这条规则也同样适用。