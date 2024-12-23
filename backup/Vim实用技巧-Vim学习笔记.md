# 学习资料
《Vim实用技巧》第二版中文版  关于vim的好书，值得反复学习。

本书源码下载: https://pragprog.com/titles/dnvim2/practical-vim-second-edition/

可用于跟着书本训练。

本文档是该书的阅读笔记，同时不定时杂糅其他Vim的资料和学习笔记，vimrc配置单独笔记记录。

其他资料包括：
- [快乐的 Linux 命令行](http://billie66.github.io/TLCL/)
- [简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)
- [无插件VIM编程技巧](https://coolshell.cn/articles/11312.html)
- [vim编辑器的分屏操作](https://blog.csdn.net/zty857016148/article/details/129957992)

## vim官网文档
```
:h vimtutor
```

## vim 复制粘贴，宿主机与虚拟机
参考：https://liushiming.cn/article/copy-and-paste-in-vim.html
```
vim --version | grep clipboard
```

+clipboard  代表支持与系统剪切板交互，-代表不支持。

安装桌面版本一般可以解决，如：
```
sudo apt install vim-gtk3 
```

设置 vim 默认使用系统剪贴板。

确定 vim 支持+clipboard后，如果想y/p直接和系统剪贴板打通，可以在~/.vimrc中加上以下配置）：
```
set clipboard^=unnamed,unnamedplus
```

其中 unnamed 代表`*寄存器`，unnamedplus 代表`+寄存器`。在 mac 系统中，两者都一样；一般在 linux 系统中`+`和`*`是不同的，通过快捷键<C+c>复制的只会覆盖`*寄存器`，但通过鼠标右键复制的，会同时覆盖`*`和`+`寄存器。（在linuxmint下面实操是如此）。在虚拟机里面，寄存器`+`是直接跟宿主机的剪切板映射的，`*`只能映射到虚拟机。综上，使用寄存器`+`才可以打通宿主机和虚拟机和vim，而且是双向打通。

然后可以通过映射键盘来使用`+寄存器`。

## vim基本配置
```
vim -u NONE -N
```

-u NONE 标志让 Vim 在启动时不加载你的vimrc，这样，你的定制 项就不会生效，插件也会被禁用。当用不加载 vimrc 文件的方式启动 
时，Vim会切换到 vi 兼容模式，这将导致很多有用的功能被禁用，-N 标志则设置 ‘nocompatible’ 选项，防止进入 vi 兼容模式。对于本书中的大多数例子来说，用 vim -u NONE –N启动 Vim应该可以确保你获得与书中的描述相符的体验，不过也有几处例外。有些 Vim的内置功能是由 Vim 脚本实现的，也就是说，只有在激活插件时， 它们才会工作。下面的文件中包含了激活 Vim 内置插件的最小配置。

![image](https://github.com/user-attachments/assets/655325e9-96e3-4c84-acbe-6af8c208c1b5)

![image](https://github.com/user-attachments/assets/b1971bce-d9d5-429f-8306-23911e4b3ad5)

**Vim为什么设定nocompatible？**

compatible这个词是“兼容的”这个意思。

nocompatible就是不兼容的。具体是不兼容什么，简单点说就是很老的vi的格式。vim是vi的扩展，nocompatible就是指vim在工作的时候不需要考虑和vi兼容。vim 是 vi 的最流行的加强版。它在 vi 的基础上增加了很多功能，但就不与 vi 完全兼容了。

set compatible 就是让 vim 关闭所有扩展的功能，尽量模拟 vi 的行为。

但这样就不应用 vim 的很多强大功能，所以一般没有什么特殊需要的话（比如执行很老的 vi 脚本），都要在 vim 的配置开始，写上 set nocompatible，关闭兼容模式。

由于这个选项是最最基础的选项，会连带很多其它选项发生变动（称作副作用），所以它必需是第一个设定的选项。

具体设置：一般在vimrc中修改set compatible为set nocompatible

**filetype plugin indent on 什么含义？**

filetype plugin indent on 

相当于

filetype on " 打开文件类型检测功能

filetype plugin on " vim加载文件类型插件

filetype indent on " 允许vim为不同类型的文件定义不同的缩进格式

## Vim的不同版本和功能列表
不同的版本和安装方式（编译参数）可能导致功能列表的差异。

查看版本和安装信息：
```
:version
:h +feature-list
```

Vim的功能集包括 small、normal、big和 huge，在现代计算机上，没理由不用 Vim 的 huge 功能集。

## 光标移动/文档移动/快捷键
![image](https://github.com/user-attachments/assets/ef30e593-d1a0-48cf-8d55-d853bcc6693d)

基于单词移动：
![image](https://github.com/user-attachments/assets/39255670-239a-4b04-81bf-7a287b2c96bd)

段落移动：
![image](https://github.com/user-attachments/assets/d0f7a690-16ea-4dc3-9d66-d61e6779a257)

删除修改一行：
![image](https://github.com/user-attachments/assets/439a5951-30a8-4d8d-ae5e-e05f86e99a6f)

撤销和重复 :

和任何其他编辑器一样，Vim 也记录了每一步操作。按` u `键可以撤销最后一次操作，而` Ctrl + r `组合键则可以重做此操作。

搜索结果高亮：

搜索过程中常用到的一个选项是` set hlsearch `可以用 `:noh` 命令清除高亮显示。
还有一个技巧是使用 `set incsearch`。这个选项会在读者还未完整输入搜索命令时，就将光标动态跳转到第一个匹配处。

关于set paste:

Vim 会在粘贴过程中自动缩进代码或添加注释。为避免这种情况，在粘贴代码之前先禁用缩进和自动注释，方法是运行`:set paste `命令，粘贴完成之后，恢复的命令为`:set nopaste`。**从 8.0 版本开始，Vim 默认启用括号化粘贴模式（bracketed paste mode），从而基本解决了这些问题。**

用`*`来搜索光标下的单词。

## 插入模式
vim的插入模式是一般编辑器的普通模式。

vim的普通模式、命令行模式、可视模式才是vim的灵魂所在。

自动补全：使用 ctrl-x + ctrl-n，进行buffer补全。

## 普通模式
![image](https://github.com/user-attachments/assets/1912a281-1781-4562-8332-5fdbdae6b40d)

![image](https://github.com/user-attachments/assets/60a26817-15d9-4815-9dc3-74e4f952a596)

J命令 用来连接当前行于下一行（合并成当前行）

gu/gU 改变当前字符，gu/gUaw 改变当前单词，gugu/gUgU 改变当前整行。

f{char} 当前行从光标往后搜索字符  ;继续上次搜索,反向/回退搜索

F{char} 当前行从光标往前面搜索 

与命令/word 搜索到结果后n是下一个结果，N是上一个结果。逻辑类似。Vim中还存在多种对应的操作。

/word命令查找全文，n下一个、N上一个结果。

dw 删除当前光标到单词尾部，db删除当前光标到单词头部，均不包括光标所在字符；daw：删除光标所在的整个单词和空格。

s命令：删除当前字符并进入insert模式编辑

`>>或者 >$ 缩进当前行 >G 缩进所有（文末）`

gg到第一行  G 到最后一行

R 命令可以由普通模式进入替换模式

可以用 vit 来选择标签里的内容。vit 可被解读为高亮选中标签内部的内容（visually select inside the tag），其中，it 命令是一种被称为文本对象（text object）的特殊动作命令。如`<a href="#">one</a>`

可视模式下的vit然后U：one变成ONE

等价于普通模式下的gUit。都是选中标签内的one。

## 可视模式
进入可视模式，小写v 字符可视模式  大写V 行可视模式  <C-v>块可视模式。

可视模式下，选中后s或者c命令是替换模式，选中什么就替换成空，然后自行编辑；

选中后r是替换每个选中的字符变成修改的字符，单个字符。

可视模式可用于编辑块文本，如:

```
var foo = 1 
var bar = 'a' 
var foobar = foo + bar
```

在每一行后加`；`号

`C-vjj$ A;ESC，`很方便高效。

## 命令行模式
命令行模式，也就是ex命令行模式

初时，先有 ed，ed 为 ex

之父，ex 为 vi 之父，而

vi为 Vim 之父。

ex命令行模式是ex 编辑器的余风遗韵。

有时insert模式下直接粘贴复制会出现格式异常，可以使用命令

`:set paste  `来进入，此时粘贴无格式异常。

全局查找并且替换：
```
:%s/Line/line/g
```

![image](https://github.com/user-attachments/assets/751a6dc1-af0d-4f4b-87a4-adfdd5358e1e)

不加%，范围是当前行，不加/g，范围是只替换查找到的第一个对象。

替换之前先确认，c参数（confirm）：
```
:%s/Line/line/gc
```

![image](https://github.com/user-attachments/assets/fccf30ca-da25-43d9-b389-6b641af41c6b)

同时编辑多个文件，vim file1.txt file2.txt

文件编辑的其他命令

切换缓冲区（文件）:buffer + 序号

实际上使用可以使用:ls 然后:buffer + 序号。

如果文件本身不多，使用:ls 然后:bn 直接下一个更快，不用记住序号。
```
bn == bnext;
bp == bprev
bf == bfirst
bl == blast
bd == bdelete (删除缓冲区）
```

命令:e filename.txt 打开另一个文件

当文件由：e 命令加载，你将无法用:n 或:N 命令来切换文件。这时要使用:buffer 命令加缓冲区号码，来切换文件。

以上切换方式，均可以使用yank来实现跨文件粘贴复制，很高效。

插入整个文件到另一个文件

在当前文件file1的普通模式中，:r file2可以直接将files2的所有内容都插入到file1光标所在行的下面。

文件另存为，使用 :w newfile，这样就保存了一份副本了。

保存以后，当前正在编辑的文件并不会切换，还是在原来的文件里面。

:num1,num2操作，可以指定范围操作，如

:2p 2d  :2,5p  :2,5d

:.,$  圆点代表当前行，$代表当前行到文本最后一行，中间逗号不能省略，否则只代表当前行。

:%代表当前文件所有行，等效于:1,$

复制和移动行：
```
:t == :co == :copy
:m == :move
```

![image](https://github.com/user-attachments/assets/a5312d7a-55d1-4420-a593-2341f59e0c61)

:t. 命令会创建一个当前行副本，另外一种做法则是用普通模式的 

复制和粘贴命令（yyp）来达到同样的效果。这两种复制当前行的技术 

有个需要关注的差别：yyp 会使用寄存器，:t. 则不会。因此，当我不 

想覆盖默认寄存器中的当前内容时，有时会使用 :t. 来复制行。

普通模式适合就近操作，ex命令可以远距离操作。

在指定范围上执行普通模式命令:normal

![image](https://github.com/user-attachments/assets/a4fb063a-07cf-44d3-a5d5-eff83ac782da)

重复上次ex命令:@:

自动补全：tab  <C-d>显示能补全的列表

把当前单词插入命令行

在 Vim 的命令行下， <C-r><C-w> 映射项会复制光标下的单词并 

把它插入命令行中。可以利用这一功能减少击键的次数。

普通模式下，按 q: 调出命令行窗口

![image](https://github.com/user-attachments/assets/e3b3fccb-9c90-4acd-a782-8ec05c501079)

当命令行窗口处于打开状态时，它会始终拥有焦点。这意味着，除 非关闭命令行窗口，否则无法切换到其他窗口。要想关闭命令行窗口， 可以执行 :q 命令（就像关闭普通 Vim 窗口那样），或是按 <CR> 。

在命令行窗口内按 <CR> 时，该命令在活动窗口的上下文中执行。活动窗口是指在调出命令窗口前，处于活动状态的那个窗口。当命令行窗口处于打开状态时，Vim 并不会提示哪个窗口是活动窗口，因此如果使用了分割窗口，就需要特别留意。

ex命令行真正执行shell命令，使用:! 如：

:!python %

%代表当前文件，python即shell命令。

:shell 直接调出整个shell，使用完之后exit，回到之前的vim状态。

把Ex命令存成脚本并加载，可以用 :source 来执行batch.vim脚本（参见 :h source）。脚本中 的每一行都会被当成一条 Ex 命令执行，就像在 Vim 的命令行中输入这 些命令一样。

:argdo命令表示对缓冲区列表里面的每个文件都执行命令：

:argdo source batch.vim

命令行模式:shell 可以打开shell子进程，exit之后回到vim原来的界面。

由于vim本身自己在shell中，所以这种方式不如直接在vim当前界面<C+z>直接后台挂起，然后fg。

分屏目录浏览：

上下打开当前目录 :He -> :Hexplore  在上面打开加感叹号!， :He!

左右打开当前目录：:Ve 全称为 :Vexplore （在左边分屏间浏览目录，要在右边则是 :Ve!）

分屏打开文件：

:sp  filename.txt  split默认是水平分割，即上下打开

:vs filename.txt vsplit是垂直分割，即左右打开

窗口跳转：
```
<C-W> 加方向键（h、j、k、l、<Left> 等等）可以在窗口之间跳转
<C-W>w 跳转到下一个（往右和往下）窗口，如果已经是右下角的窗口，则跳转到左上角的窗口
<C-W>W 跳转到上一个（往左和往上）窗口，如果已经是左上角的窗口，则跳转到右下角的窗口
<C-W>n 或 :new 打开一个新窗口
<C-W>c 或 :close 关闭当前窗口；当前窗口如果已经是最后一个则无效
<C-W>q 或 :quit 退出当前窗口，当最后一个窗口退出时则退出 Vim
<C-W>o 或 :only 只保留当前窗口，关闭其他所有窗口
<C-W>s 和 :split 作用相同，把当前窗口横向一分为二
<C-W>v 和 :vsplit 作用相同，把当前窗口纵向一分为二
<C-W>= 使得所有窗口大小相同（当调整过终端或图形界面 Vim 的窗口大小后特别有用）
<C-W>_ 设置窗口高度，命令前的数字表示高度行数，默认为纵向占满（想专心编辑某个文件时很有用）
<C-W>| 设置窗口宽度，命令前的数字表示宽度列数，默认为横向占满
<C-W>+ 增加窗口的高度，命令前的数字表示需要增加的行数，默认为 1
<C-W>- 减少窗口的高度，命令前的数字表示需要减少的行数，默认为 1
<C-W>> 增加窗口的宽度，命令前的数字表示需要增加的列数，默认为 1
<C-W><lt>（提醒，我们用 <lt> 表示“<”键）减少窗口的宽度，命令前的数字表示需要增加的列数，默认为 1
```

标签页跳转：
```
在已有命令行模式命令前加 tab␣ 可以在新标签页中展示命令的结果，如 :tab help
可以在新标签页中打开帮助，:tab split 可以在新标签页中打开当前缓冲区
:tabs 展示所有标签页的列表
:tabnew 或 :tabedit 可以打开一个空白的新标签页，后面有文件名的话则打开该文件
:tabclose 可以关闭当前标签页（如果标签页里只有一个窗口，使用窗口关闭命令<C-W>c 应该更快）
:tabnext、gt 或 <C-PageDown> 可以切换到下一个标签页
:tabNext、:tabprevious 、gT 或 <C-PageUp> 可以切换到上一个标签页
:tabfirst 或 :tabrewind 切换到第一个标签页
:tablast 切换到最后一个标签页
```