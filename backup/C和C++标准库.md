## 标准库相关文档资料
比较有权威参考性的网址：
- https://pubs.opengroup.org/onlinepubs/9699919799/
- https://www.cprogramming.com/
- https://en.cppreference.com/w/
- http://www.crasseux.com/books/ctutorial/

## 标准库的历史
随着语言的发展新的papers（指官方的叫标准的书）会被发布，每一次都定义一个新的标准。这就是为什么我们会有不同的C和C++版本的原因：C99, C11, C++03, C++11, C++14等等，数字与出版/发布年份相符。

这些标准都市非常详细和有技术新的文档：我不会把它们当作手册。通常会分为两部分：
- C/C++的功能和特性；
- C/C++的API--开发人员可以用于他们的C/C++程序的一个类、函数和宏的集合。它也被称为标准库。

C标准库也称为ISO C库，是用于完成诸如输入/输出处理、字符串处理、内存管理、数学计算和许多其他操作系统服务等任务的宏、类型和函数的集合。它是在C标准中（例如C11标准）中定义的。

举例，以c标准库历史介绍：
- [ANSI C](https://zh.wikipedia.org/wiki/ANSI_C)共包括15个头文件。
- 1995年，Normative Addendum 1 （NA1）批准了3个头文件（iso646.h、wchar.h和wctype.h）增加到C标准函数库中。
- [C99](https://zh.wikipedia.org/wiki/C99)标准增加了6个头文件（complex.h、fenv.h、inttypes.h、stdbool.h、stdint.h和tgmath.h）。
- [C11](https://zh.wikipedia.org/wiki/C11)标准中又新增了5个头文件（stdalign.h、stdatomic.h、stdnoreturn.h、threads.h和uchar.h）。

至此，C标准函数库共有29个头文件。

![image](https://github.com/user-attachments/assets/490185ad-3aa9-4dee-85ba-12834a53d02c)

我们从这里开始讨论真正的代码了。从事于标准库实现的开发者阅读官方的ISO规范并将其转化为代码。他们必须依赖其操作系统所提供的功能（读/写文件，分配内存，创建线程，......所有这些被称为系统调用），因此每个平台都有其自己的标准库实现。有时它是系统内核的一部分，有时它是作为一个附加组件 - 编译器 - 必须单独下载。

和C标准库的概念类似，但仅针对C ++。C++标准库是一组C++模板类，它提供了通用的编程数据结构和函数，如链表、堆、数组、算法、迭代器和任何其他你可以想到的C++组件。C ++标准库也包含了C标准库，并在C++标准中进行了定义（例如C++ 11标准）。

**GNU C库，也称为glibc, 是C标准库的GNU项目实现。**并非所有的标准C函数都可以在glibc中找到：大多数数学函数实际上是在libm库中实现的，这是一个独立的库。截至今天，glibc是Linux上使用最广泛的C库。然而，在90年代期间，有一段时间里，glibc有一个竞争对手称为Linux libc（或者简称libc），它是由glibc 1.x的一个分支产生的。在一段时间里，Linux libc是许多Linux发行版中的标准C库。

**经过多年的发展，glibc竟然比Linux libc更具优势，并且所有使用它的Linux发行版都切换回了glibc。**所以，如果你在你的磁盘中找到一个名为**libc.so.6**的文件，请不要担心：它是现代版的glibc。

为了避免与之前的Linux libc版本混淆，版本号增加到了6（他们无法将其命名为glibc.so.6：所有Linux库都必须以lib前缀打头）。

在Linux系统下，C语言标准库通常位于以下几个目录中：
- /usr/include：这个目录包含了C标准库的头文件（即.h文件），这些头文件定义了库函数的声明和宏定义。
- /usr/lib：这个目录包含了C标准库的编译好的对象文件（即.a文件，静态库）和共享对象文件（即.so文件，动态库）。静态库在程序编译时链接，而动态库在程序运行时链接。
- /usr/lib64：在64位Linux系统上，一些库可能位于这个目录，特别是64位版本的库。

例如，标准输入输出库stdio.h的头文件通常位于/usr/include/stdio.h，而对应的库文件可能位于/usr/lib/libc.so（动态库或/usr/lib/libc.a（静态库）。要查找特定库文件的位置，可以使用find或locate命令。例如，要查找标准C库libc的共享对象文件。

静态库在程序编译时链接，而动态库在程序运行时链接。

linuxmint上，c标准库路径：/usr/lib/x86_64-linux-gnu/libc.*

包括静态库和动态库。

libc.a  libc.so  libc.so.6

![image](https://github.com/user-attachments/assets/3bda8274-3813-4299-be92-0c157ec329d2)

![image](https://github.com/user-attachments/assets/8c4a6e81-e348-494e-8c62-bce844c56b15)

也可以通过命令查出本机的标准库位置：

ldconfig -p | grep libc.so

![image](https://github.com/user-attachments/assets/667003d9-609f-481e-86ee-39c75e354bce)

在大多数Linux系统中，libc.so.6是默认的C库。

查看标准库版本：

![image](https://github.com/user-attachments/assets/fa3285e1-279b-494f-87dd-a44c587f5eea)

另一方面，C++标准库的实现位于libstdc++或GNU标准C++库中。这是一个正在进行的在GNU/Linux上实现标准C++库的项目。一般来说，所有常规的Linux发行版都默认使用libstdc++。

在Mac和iOS上，C标准库的实现是libSystem的一部分，libSystem是位于/usr/lib/libSystem.dylib中的核心库。LibSystem包含其他组件，如数学库、线程库和其他底层实用程序。关于C++标准库，在OS X Mavericks（V10.9）之前的Mac上，libstdc++是默认选项。这在现代的基于Linux的系统上可以找到的同样的实现。自OS X Mavericks开始，Apple切换到使用libc++，这是LLVM项目，替代了GNU libstdc++标准库。IOS开发者可以使用iOS SDK（软件开发工具包）来访问标准库，它是一系列允许创建移动应用程序的工具。

在Windows上，标准库的实现一直严格限定在Visual Studio中，它是微软官方的编译器。

他们通常称之为C/C++运行时库（CRT），并且它涵盖了c/c++二者的实现。

Bionic是Google为其Android操作系统所编写的C标准库实现，它直接在底层使用。

第三方开发者可以通过Android原生开发工具包（NDK）访问Bionic，该工具集允许你使用C和C++代码编写Android应用程序。

在 C++ 端, NDK提供了很多版本的实现：
- libc++，从Lollipop开始的官方安卓系统和现代Mac操作系统都将其作为C++标准库使用。从NDK发布17版本开始，它将成为NDK中唯一可用的C++标准库实现；
- gnustl，libstdc++的别名，这两者在GNU/linux是同一个库。这个库的已被弃用，它将在NDK发布18中删除；
- STLport，由STLport项目编写的C++标准库的第三方实现，自2008年以来一直处于不活跃状态。与gnustl一样，STLport将在NDK发布18中移除。
