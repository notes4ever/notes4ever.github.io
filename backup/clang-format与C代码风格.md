## clang-format
官网文档：
- https://clang.llvm.org/docs/ClangFormat.html
- https://clang.llvm.org/docs/ClangFormatStyleOptions.html

各个配置中文解释:
- https://blog.csdn.net/u013576331/article/details/135011351
- https://blog.csdn.net/softimite_zifeng/article/details/78357898
- https://blog.csdn.net/lishi_1991/article/details/135565891

在线预览配置的网站：https://clang-format-configurator.site

使用clang-format生产配置文件：
```
clang-format -style=llvm -dump-config > .clang-format
```

## 主流C代码风格
Linux 内核代码风格，可以多读几遍，重点参考：
- https://www.kernel.org/doc/html/v5.9/translations/zh_CN/process/coding-style.html

![image](https://github.com/user-attachments/assets/ebe8a315-c4ee-4fd5-a807-16dd45e9f6ef)

google的风格：
- https://zh-google-styleguide.readthedocs.io/en/latest/
- https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/formatting.html

还有其他的代码风格，llvm等等。

K&R 很重要，代码基本都是大家参考的对象。

## K&R 主要风格摘要
![image](https://github.com/user-attachments/assets/e5e25424-d3a7-4c7e-8efc-61d8ca09ac4d)

来自k&r 第二版。除了上面所表示的容易出错的风格，其他的花括号，k&r风格总是在if  for  while等，只要下面最多一个语句块，即（最多一个块，或者一行语句以分号;结尾），后面省略花括号，如果超过1个，那就加上花括号。简洁为主。比如下面的风格：
![image](https://github.com/user-attachments/assets/2a48db98-14f9-460b-8989-62e4cb470c37)

kernel风格与K&R风格的case处理一致，都是不缩进，并且case后面冒号没有花括号。个人总结，case缩进不缩进都可以。既然无所谓还是与大佬观点保持一致。
![image](https://github.com/user-attachments/assets/89f5f0e9-d0e1-4378-a78f-a077535b0ee5)

K&R的if与else处理：
![image](https://github.com/user-attachments/assets/2a47ea45-5804-4607-a46e-a75a888bff07)

其他风格比如google和内核风格是不鼓励这种，if和else应该保持一致，都有花括号。个人总结：倾向google和内核的方式。
![image](https://github.com/user-attachments/assets/7343307a-7cd0-4b5f-a122-2b76f23dbd14)

K&R while有时加花括号，有时不加，没有固定套路。
![image](https://github.com/user-attachments/assets/3fd6653f-4ee8-4f97-9f32-9e5fcfc392dc)

很多地方都是，有的加有的不加。

![image](https://github.com/user-attachments/assets/5debcb91-3367-4768-8042-19b5da86fab2)

![image](https://github.com/user-attachments/assets/7e5da943-c0f9-4ff1-a50f-7ec0bef22164)

![image](https://github.com/user-attachments/assets/27807845-df08-4c6e-bead-33939cd01ff5)

个人总结：while里面内容稍微多点的话，加上花括号肯定比不加更容易阅读。

![image](https://github.com/user-attachments/assets/96f5be78-ae4d-4db1-a8dd-42934b16e6f3)

K&R和kernel风格都不推荐一行放多个语句，比如if return等，再怎么短的语句都不会放到一行。google风格会放到一行。个人总结：还是遵从前者，没必要为了代码看起来紧凑，牺牲代码可读的一致性。


## 自己的代码风格
同时参考linux kernel和llvm，最终形成自己的配置.clang-format:
```
# clang-format官方参数配置说明：
# https://clang.llvm.org/docs/ClangFormatStyleOptions.html
# linux kernel代码风格说明：
# https://www.kernel.org/doc/html/v5.9/translations/zh_CN/process/coding-style.html
###### 基于LLVM的默认配置上修改
BasedOnStyle: LLVM
# 最大空行保持
MaxEmptyLinesToKeep: 3
# c++11 c++14 c++17 c++20等
Standard: Latest
###### tab 空格 缩进
# 是否使用tab 需要配合vscode的insertSpaces配置
# llvm的风格：Never linux的风格：Always
UseTab: Never
# 缩进宽度
# llvm默认2空格 linux是tab缩进 默认是8
# 2太短 8太宽 取折中4
IndentWidth: 4
# tab的宽度 llvm和linux风格默认都是设置8
# 改成与缩进宽度一样的4
TabWidth: 4
###### 使用linux风格的配置
# 函数大括号换行
BreakBeforeBraces: Linux
# case是否缩进 linux风格是不缩进
IndentCaseLabels: false
# 允许短的函数放在同一行
# None, InlineOnly(定义在类中), Empty(空函数),
# Inline(定义在类中，空函数), All
#  linux风格是None llvm风格是All
AllowShortFunctionsOnASingleLine: None
```
