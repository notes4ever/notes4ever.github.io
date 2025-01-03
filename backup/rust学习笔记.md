## 陈天Rust编程第一课
前置知识 堆栈内存：

对于存入栈上的值，它的大小在编译期就需要确定。栈上存储的变量生命周期在当前调用 栈的作用域内，无法跨调用栈引用。

堆可以存入大小未知或者动态伸缩的数据类型（编译期间不确定，比如多线程使用的数据结构）。堆上存储的变量，其生命周期从分配后开始，一直到释放时才结束，因此堆上的变量允许在多个调用栈之间引用。但也导致堆变量 的管理非常复杂，手工管理会引发很多内存安全性问题，而自动管理，无论是 GC 还是 ARC，都有性能损耗和其它问题。

栈上存放的数据是静态的，静态大小，静态生命周期；堆上存放的 数据是动态的，动态大小，动态生命周期。

编程开发基本概念：

- 内存——堆栈内存
- 数据——程序操作的对象，值、类型（基础原生类型与复杂组合类型）、指针与引用
- 代码——程序运行的主体，函数、方法、闭包、接口
- 运行方式——程序的执行效率，并发、并行、同步、异步
- 编程范式——提升代码的质量：泛型编程、数据机构+代码

指针和引用都指向值的内存地址，只不过二者在解引用时的行为不一样。引用只能解引用到原来的数据类型，而指针没有这个限制，然而，不受约束的指针解引用，会带来内存安全方面的问题。

泛型编程通过参数化让数据结构像函数一样延迟绑定，提升其通用性，类型的参数可以用 接口约束，使类型满足一定的行为，同时，在使用泛型结构时，我们的代码也需要更高的 抽象度。

Rust语言基本知识，摘要与笔记：

![image](https://github.com/user-attachments/assets/a5da8f1f-2e57-40c2-8afa-feb8daed263d)

Rust 的模式匹配是一个很重要的语言特性，被广泛应用在状态机处理、消息处理和错误处 理中，如果你之前使用的语言是 C / Java / Python / JavaScript ，没有强大的模式匹配支 持，要好好练习这一块。

很多语言的 while loop，for loop，if else 都 是语句，但 Rust 下它们是表达式，它们会返回最后一个表达式的值（即便是 unit）。 let/static/const/fn 这样的定义，它们是纯粹的语句。

定义在 impl 中且没有 self 的函数被称之为关联函数： 因为它没有 self，不能用 f.read() 的形式调用，因此它是一个函数而不是方法，它又在 impl 中，与结构体紧密关联，因此称为关联函数。

与圣经一起，了解了rust基础知识。

## 《Rust程序设计语言》《Rust圣经》
230827开始学习这两本书

- 《Rust程序设计语言》
- 《Rust语言圣经》本书确实称得上是圣经级别的入门书。作者很热爱Rust，这部分写得诙谐又激情，介绍了Rust的特点，鼓励大家认真研读本书，避免从入门到放弃，同时介绍了Rust社区与一些补充学习的资料。

Rust语言基础学习部分，泛型和特征trait，学习到这，这一节很难，没看懂，晦涩难懂。Rust学习跟Vim类似，坡度比较陡峭，跟Python完全相反，非常不利于推广，对编程者不友好，但是对产品用户比较友好。

## 其他笔记
查看已安装的rust：
```
rustup show
```

![image](https://github.com/user-attachments/assets/0d54d529-7c7c-411e-95c8-075198ccf968)

![image](https://github.com/user-attachments/assets/4e1fc85c-a220-452c-85a4-f18609f5a295)

vscode：

只需要安装一个rust插件：rust-analyzer

Error Lens 可选

依赖的插件：

![image](https://github.com/user-attachments/assets/3f8feac0-d950-4dd0-ba2e-e340d83c57b4)

国内源设置：

![image](https://github.com/user-attachments/assets/ae752cf5-07e9-41a2-b434-7d22728319e9)

https://rsproxy.cn/

![image](https://github.com/user-attachments/assets/05f906e3-fe92-4ddf-b2f7-456023778ab1)

![image](https://github.com/user-attachments/assets/a4a9a214-d9a7-41b0-8c16-a26f4e1bc33b)

![image](https://github.com/user-attachments/assets/811b8007-1204-4c62-b5a2-cde6e8067436)

![image](https://github.com/user-attachments/assets/4fb147a2-c70f-436e-8908-3e18600b9ed3)

![image](https://github.com/user-attachments/assets/1bc0bc2d-dc72-4e20-a616-7480bc91761b)

![image](https://github.com/user-attachments/assets/d719b69d-7add-45d8-99df-222f3ef6c2ab)

![image](https://github.com/user-attachments/assets/61290638-3a69-4dca-b981-13e05327d518)
