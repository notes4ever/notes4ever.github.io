```
""""""必需的设置 vim最小化配置
set nocompatible " 不兼容vi
filetype plugin indent on " 激活Vim内置文件类型插件 一般还会加indent启用根据文件类型缩进
syntax on " 语法高亮

""""""复制粘贴 剪切板 解释与配置
"vim --version | grep clipboard
"+clipboard  代表支持与系统剪切板交互，-代表不支持。
"unnamed 代表*寄存器，unnamedplus 代表+寄存器。在 mac 系统中，两者都一样。
"linux系统中+和*是不同的，通过快捷键<C+c>复制的只会覆盖"*寄存器，但通过鼠标右键复制的，
"会同时覆盖"*和"+寄存器。虚拟机里，寄存器"+直接跟宿主机的剪切板映射,"*只能映射到虚拟机。
"综上，使用寄存器"+才可以同时打通宿主机和虚拟机和vim，而且是双向打通。
set clipboard^=unnamed,unnamedplus       " 寄存器*与+ :reg可查看寄存器里的内容

""""""快捷键映射
let mapleader=";"       " 默认是\ 例如格式化代码是\p
set timeoutlen=1500     " leader按键最迟1.5秒不按下一个就失效
"""普通模式下的键盘映射
nnoremap <Leader><Leader> "+     " 剪切板 使用: yy;;
nnoremap <tab> <C-w>w            " 分屏之间切换
nnoremap <Leader>r <C-r>         " redo 重做，即撤销u的反面
nnoremap <Leader>d <C-d>         " 向下半页
nnoremap <Leader>u <C-u>         " 向上半页
nnoremap <Leader>f <C-f>         " 向前一页
nnoremap <Leader>b <C-b>         " 退后一页

""""""设置空格 制表符宽度
"如需统一空格与tab，命令:retab 可以把空格转换成tab或者tab转成空格
"set noexpandtab   " 制表符不自动转换成空格
set expandtab      " 新输入的tab自动转换成空格 之前的tab转换成空格 使用:retab命令
set tabstop=4      " 制表符宽度 默认8
set softtabstop=4
set shiftwidth=4
set autoindent     " 开始新行时处理缩进 沿用上一行缩进
set list listchars=tab:▸\                    " 只显示tab
"set list listchars=tab:▸\ ,trail:•,eol:¬    " 显示制表符 行尾空格 回车

""""""文件相关配置 撤销 备份 重载等
set undofile
" 硬编码 如果undodir目录不存在，须先创建
set undodir=~/.vim/undodir
if !isdirectory(&undodir)
    call mkdir(&undodir, 'p', 0700)
endif
set noswapfile      " 不生成交换文件.filename.swp  配合undofile
set autoread        " 从磁盘自动重载文件
set nobackup        " 不生成备份文件filename~  默认就是关闭的
set wildmenu        " 增强命令行补全功能  展示match的文件列表
set path+=**        " 递归查找子文件
set encoding=utf-8  " 文件默认编码
set fileencodings=utf-8,gb18030,latin1

""""""主题与字体
if (has('termguicolors'))
    set termguicolors
endif
" 硬编码 ~/.vim/colors需要先存在主题
colorscheme Tomorrow-Night-Bright 
set t_Co=256

""""""其他配置
set sidescroll=1        " 更平滑的侧边滚动条
set laststatus=2        " 总是显示状态栏
set backspace=2         " insert模式下用退格键删除 现代编辑器的退格键行为
set history=10000       " 最大的历史记录数
set number              " 显示行数
set ruler               " 显示尺度信息 (在右下部位显示行、列和百分位)
set cursorline          " 高亮光标所在行
set noerrorbells        " 不让vim发出讨厌的滴滴声
"set hlsearch            " 可以用:noh 命令清除高亮显示 如果主题颜色异常需要关闭
set incsearch           " 搜索时 边输入边搜索、并移动光标
set textwidth=80       " 设置文本宽度
set colorcolumn=80     " 在第N列显示一个竖线
set wrap                " 设置自动换行
set linebreak           " 设置单词边界换行
```
