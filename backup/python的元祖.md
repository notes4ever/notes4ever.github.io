```
# 元祖初始化方法1：使用字面量语法定义元组
>>> t = (0, 1, 2)
```

真相：“括号”其实不是定义元组的关键标志——直接删掉两侧括号同样也能完成定义，“逗号”才是让解释器判定为元组的关键。
```
>>> t = 0, 1, 2
>>> t
```

```
# 元祖初始化方法2：使用 tuple(iterable) 
# 内置函数初始化
>>> t = tuple('foo')
>>> t
('f', 'o', 'o')
```

在 Python 中，函数可以一次返回多个结果，这其实是通过返回一个元组来实现的。

```
>>> results = tuple((n * 100 for n in range(10) if n % 2 == 0))
>>> results
(0, 200, 400, 600, 800)
```

（n * 100 for n in range(10) if n % 2 == 0)并没有生成元组，而是返回了一个生成器（generator）对象。因此它是生成器推导式，而非元组推导式。但生成器仍然是一种可迭代类型，所以我们还是可以对它调用 tuple() 函数，获得元组。

元组经常用来存放结构化数据，但只能通过数字来访问元组成员其实特别不方便；为了解决这个问题，我们可以使用一种特殊的元组：
具名元组（namedtuple）。具名元组在保留普通元组功能的基础上，允许为元组的每个成员命名，这样你便能通过名称而不止是数字索引访问成员。

```
from collections import namedtuple

Rectangle = namedtuple('Rectangle', 'width,height')
```

在 Python 3.6 版本以后，除了使用 namedtuple() 函数以外，你还可以用 typing.NamedTuple 和类型注解语法来定义具名元组类型。这种方式在可读性上更胜一筹：
```
class Rectangle(NamedTuple):
    width: int
    height: int

rect = Rectangle(100, 20)
```

但需要注意的是，上面的写法虽然给 width和 height 加了类型注解，但 Python 在执行时并不会做真正的类型校验。