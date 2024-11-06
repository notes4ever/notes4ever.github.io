> **tee命令：读取标准输入内容（包括缓冲区比如vim编辑刷新内容），将读取到的数据写到标准输出和文件。**

## 基本用法

`echo "Hello, Linux!" | tee output.txt
`

没有-a参数，表示直接覆盖output.txt里面的内容。

`echo "Appended Text" | tee -a output.txt
`

-a：追加到文件而不是覆盖。使用该选项，可以将输出内容追加到指定文件的末尾。

适合用来添加内容到文件，比如日志等：

```
some_command | tee -a operation.log
command_with_error 2>&1 | tee output.txt
echo "Data" | sudo tee file.txt > /dev/null
```


同时写入多个文件：

`echo "Data" | tee file1.txt file2.txt file3.txt
`

同时查看日志文件的变化并保存到文件：

`tail -f /var/log/syslog | tee syslog_monitor.log
`

这个例子中，tail -f用于实时跟踪日志文件的变化，而tee则将输出同时显示在终端并保存到syslog_monitor.log文件中。
