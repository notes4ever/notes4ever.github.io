![image](https://github.com/user-attachments/assets/40403db6-e82d-4a8b-9469-52cbf3a57397)

在大多数 Linux 系统中，/etc/shells 文件中列出了各种已安装的 shell。

查看默认的shell：

cat /etc/passwd

也可以使用neofetch查看。

也可以echo $0查看当前shell。

在现代 Linux 系统中， /bin 目录通常是/usr/bin/目录的符号链接， 这就是为什么用户 christine 的默认 shell 程序是/bin/bash，但 bash shell 程序实际位于/usr/bin/目录。

![image](https://github.com/user-attachments/assets/84460175-7736-462f-9e07-d7113536bcc3)

对 bash shell 脚本来说，这两种 shell（默认的交互 shell 和默认的系统 shell）可能会导致问题。熟悉有关 bash shell 脚本首行的语法要求，以避免这些麻烦。

并不是非得使用默认的交互式 shell。可以启动任意一种已安装的 shell，只需输入其名称即可。但屏幕上不会有任何提示或消息表明你当前使用的是哪种 shell。

$0 变量可以助你一臂之力。 命令 echo $0 会显示当前 shell 的名称，提供必要的参考。

使用 echo $0 显示当前所用 shell 的做法仅限在 shell 命令行中使用。如果在 shell 脚本中使用，则显示的是该脚本的名称。

![image](https://github.com/user-attachments/assets/d309eaed-96dc-480c-bb85-6031b93c0938)
