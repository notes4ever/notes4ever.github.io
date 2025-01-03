集合是一种不重复、无序的可变容器类型。
```
>>> fruits = {'apple', 'orange', 'apple', 'pineapple'}
```

重新查看上面 fruits 变量的值，你会马上体会到集合最重要的两个特征——去重与无序——重复的 'apple' 消失了，成员顺序也被打乱了：
```
>>> fruits
{'pineapple', 'orange', 'apple'}
```

所以使用集合实现去重，得到的结果会丢失集合内成员原有的顺序。

如果你既需要去重，又想要保留原有顺序，怎么办？可以使用前文提到过的有序字典 OrderedDict 来完成这件事。因为 OrderedDict 同时满足两个条件：

- 它的键是有序的；
- 它的键绝对不会重复。

因此，只要根据列表构建一个字典，字典的所有键就是有序去重的结果：
```
>>> from collections import OrderedDict
>>> list(OrderedDict.fromkeys(nums).keys()) ➊
[10, 2, 3, 21]
```

调用 fromkeys 方法会创建一个有序字典对象。字典的键来自方法的第一个参数：可迭代对象（此处为 nums 列表），字典的值默认为 None。

要初始化一个空集合，只能调用 set() 方法，因为 {} 表示的是一个空字典，而不是一个空集合。
```
# 正确初始化一个空集合
>>> empty_set = set()
```

集合也有自己的推导式语法：
```
>>> nums = [1, 2, 2, 4, 1]
>>> {n for n in nums if n < 3}
{1, 2}
```

frozenset集合是一种可变类型，使用 .add() 方法可以向集合追加新成员：
```
>>> new_set = set(['foo', 'foo', 'bar'])
>>> new_set.add('apple')
>>> new_set
{'apple', 'bar', 'foo'}
```

假如你想要一个不可变的集合，可使用内置类型 frozenset，它和普通 set 非常像，只是少了所有的修改类方法：
```
>>> f_set = frozenset(['foo', 'bar'])
>>> f_set.add('apple')
# 报错：没有 add/remove 那些修改集合的方法
```

假如你想要一个不可变的集合，可使用内置类型 frozenset，它和普通 set 非常像，只是少了所有的修改类方法：
```
>>> f_set = frozenset(['foo', 'bar'])
>>> f_set.add('apple')
# 报错：没有 add/remove 那些修改集合的方法
AttributeError: 'frozenset' object has no attribute 'add'
```

**集合只能存放可哈希对象。**

比如下面的集合可以被成功初始化：
```
>>> valid_set = {'apple', 30, 1.3, ('foo',)}
```

但这个集合就不行：
```
>>> invalid_set = {'foo', [1, 2, 3]}
...
TypeError: unhashable type: 'list'
```

正如上面的报错信息所示，集合里只能存放“可哈希”（hashable）的对象。假如把不可哈希的对象（比如上面的列表）放入集合，程序就会抛出 TypeError 异常。

首先，那些**不可变的内置类型**都是可哈希的：
```
>>> hash('string')
-3407286361374970639
>>> hash(100)
# 有趣的事情，整型的 hash 值就是它自身的值
100
>>> hash((1, 2, 3))
529344067295497451
```

而可变的内置类型都无法正常计算哈希值：
```
>>> hash({'key': 'value'})
TypeError: unhashable type: 'dict'

>>> hash([1, 2, 3])
TypeError: unhashable type: 'list'
```

可变类型的不可哈希特点有一定的“传染性”。比如在一个原本可哈希的元组里放入可变的列表对象后，它也会马上变得不可哈希：
```
>>> hash((1, 2, 3, ['foo', 'bar']))
TypeError: unhashable type: 'list'
```

由用户定义的所有对象默认都是可哈希的：
```
>>> class Foo:
...     pass
...
>>> foo = Foo()
>>> hash(foo)
273594269
```

总结一下，某种类型是否可哈希遵循下面的规则：

- 所有的不可变内置类型，都是可哈希的，比如 str、int、tuple、frozenset 等；
- 所有的可变内置类型，都是不可哈希的，比如 dict、list 等；
- 对于不可变容器类型 (tuple, frozenset)，仅当它的所有成员都不可变时，它自身才是可哈希的；
- 用户定义的类型默认都是可哈希的。

谨记，**只有可哈希的对象，才能放进集合或作为字典的键使用**。