目的是要生成apue.h头文件和静态链接库libapue.a

qpue.h是所有函数的声明以及宏定义，gcc在搜索头文件的时候默认会去搜索/usr/include/中的文件。

libapue.a是apue.h头文件中包含的所有函数及宏定义的具体实现，是一个静态链接库，gcc在搜索链接库的时候默认会去搜索/usr/local/lib/中的文件。

**gcc 如何使用静态库和动态库**：

编译时可以直接带上完整路径的链接库进行编译，如 g++ main.cpp lib/libmyfun.so -o main ，但更多的是使用下面的方法。

gcc 使用 -l 参数来指定链接库，如链接 libtest.a 或 libtest.so，使用 -ltest。使用 -l 链接时去除 lib 前缀和后缀即可。

编译时，默认的链接库目录为 /usr/lib && /usr/local/lib ,如果是64位系统，还有 /usr/lib64；否则就需要通过 -L 参数来指定自定义链接库目录。(注：有的LINUX发行版好像不支持 /usr/local/lib 和 /usr/local/lib64，所以最好是使用 /usr/lib)

静态库链接后，是将代码直接嵌入到可执行文件中，所以运行时，不需要再指定静态库；而动态库则不然。如果动态库位于系统默认的库目录，则可直接运行。

以上说明参考：https://www.cnblogs.com/tianyajuanke/p/3359100.html

下面是编译过程记录：

sudo apt-get install libbsd-dev

解压官网下载的源码，进入文件夹make 

碰到错误1：

```
make[1]: 进入目录“/media/sf_share/src3e/apue.3e/db”
gcc -fPIC -ansi -I../include -Wall -DLINUX -D_GNU_SOURCE  -c db.c
gcc -shared -Wl,-dylib -o libapue_db.so.1 -L../lib -lapue -lc db.o
/usr/bin/ld: Error: unable to disambiguate: -dylib (did you mean --dylib ?)
collect2: error: ld returned 1 exit status
make[1]: *** [Makefile:32：libapue_db.so.1] 错误 1
make[1]: 离开目录“/media/sf_share/src3e/apue.3e/db”
make: *** [Makefile:6：all] 错误 1
```

解决：

修改db/Makefile 
```
gcc -shared -Wl,-dylib -o libapue_db.so.1 -L../lib -lapue -lc db.o
```

改成

```
gcc -shared libapue_db.so.1 -L../lib -lapue -lc db.o
```

然后报错找不到libapue_db.so.1

直接删除db文件夹，再去掉根目录下Makefile中db目录，重新make

碰到错误2：

```
gcc -ansi -I../include -Wall -DLINUX -D_GNU_SOURCE  devrdev.c -o devrdev  -L../lib -lapue 
devrdev.c: In function ‘main’:
/usr/bin/ld: /tmp/cc40xX6i.o: in function `main':
devrdev.c:(.text+0xcb): undefined reference to `minor'
/usr/bin/ld: devrdev.c:(.text+0xe1): undefined reference to `major'
/usr/bin/ld: devrdev.c:(.text+0x131): undefined reference to `minor'
/usr/bin/ld: devrdev.c:(.text+0x147): undefined reference to `major'
collect2: error: ld returned 1 exit status
make[1]: *** [Makefile:18：devrdev] 错误 1
make[1]: 离开目录“/media/sf_share/src3e/apue.3e/filedir”
make: *** [Makefile:6：all] 错误 1
```

解决：在出错的文件devrdev.c，加入以下代码
```
#include <sys/types.h>
#include <sys/sysmacros.h>
```

碰到错误3：

```
make[1]: 进入目录“/media/sf_share/src3e/apue.3e/stdio”
gcc -ansi -I../include -Wall -DLINUX -D_GNU_SOURCE  buf.c -o buf  -L../lib -lapue 
buf.c: In function ‘is_unbuffered’:
buf.c:90:15: error: ‘FILE’ has no member named ‘__pad’; did you mean ‘__pad5’?
buf.c: In function ‘buffer_size’:
buf.c:92:15: error: ‘FILE’ has no member named ‘__pad’; did you mean ‘__pad5’?
```

参考：https://blog.csdn.net/hp_cpp/article/details/110330825

stdio/but.c 将这两处的_flag改成_flags，但这种方案测试无法解决；最后直接删除stdio/buf stdio/buf.c 修改stdio/Makefile 去掉buf后重新编译，有警告但是成功了。

编译成功后：
```
cp ./include/apue.h /usr/include/  
cp ./lib/error.c /usr/include
cp ./lib/libapue.a /usr/local/lib/
```

在apue.h最后加一行代码
```
#include "error.c"
```

其他参考：

- https://www.cnblogs.com/ch122633/p/7928465.html
- https://www.cnblogs.com/howe670/p/8732933.html

```
sudo cp ./include/apue.h /usr/include/
sudo cp ./lib/libapue.a /usr/local/lib/
```

上面这两行实际在mint上执行过;如果文件权限有问题，可以修改修复。
```
sudo chmod 644 apue.h
sudo chmod 644 libapue.a
```

Macos上，clang路径不一样。make顺利通过编译，然后.a可以直接像linux那样复制过去。
```
sudo cp ./lib/libapue.a /usr/local/lib/
```

但mac上面没有/usr/include,然后试了网上说的路径：
```
sudo cp apue.h /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/
```

也不行，所以直接复制apue.h 头文件到本地项目文件就可以了，跟源码放一起，不必放到系统默认库的路径下。使用示例：

```
gcc filename.c -lapue
```