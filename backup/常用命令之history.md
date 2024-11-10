![image](https://github.com/user-attachments/assets/791373b1-8bc8-4c3a-be44-bf55eb6fa583)

history 是一个实用的内建命令，能帮助你管理先前执行过的命令。

历史记录中通常默认保存最近的 1000 条命令。可以设置保存在 bash 历史记录中的命令数量。为此，需要修改名为 HISTSIZE 的环境变量。

当输入!时，bash 会先显示从 shell 的历史记录中唤回的命令，然后再执行该命令。 命令历史记录被保存在位于用户主目录的隐藏文件.bash_history 之中。

！+ 行号 是执行第几行的的命令。

！！是执行历史记录中最后一条命令。

这里要注意的是，在 CLI 会话期间，bash 命令的历史记录默认被保存在内存中。当 shell 退出时 才被写入历史文件。
可以在不退出 shell 的情况下强制将命令历史记录写入.bash_history 文件。为此，需要使用 history 命令的-a 选项。

**history -a**

如果打开了多个终端会话，则仍然可以使用 history -a 命令在每个打开的会话中向 .bash_history 文件添加记录。

但是历史记录并不会在其他打开的终端会话中自动更新。 

这是因为.bash_history 文件只在首次启动终端会话的时候才会被读取。

要想强制重新读取.bash_history 文件，更新内存中的终端会话历史记录，可以使用` history -n `命令。