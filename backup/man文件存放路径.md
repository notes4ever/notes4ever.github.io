## man文件存放路径
man命令通过环境变量`$MANPATH`知道：

`echo $MANPATH`

## 手动存放man文件
知道man路径后，手动存放，比如：

`/usr/local/share/man/man1
`

然后二进制命令xxx存放：

`/usr/local/bin
`

`man xxx `就有文档了。
