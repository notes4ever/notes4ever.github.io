## 全局变量
全局环境变量对于 shell 会话和所有生成的子 shell 都是可见的。

局部环境变量则只对创建它 的 shell 可见，子shell无法继承。

如果程序创建的子 shell 需要获取父 shell 信息，那么全局环境变量就能派上用场了。

查看全局变量：
```
printenv 
```
或者：
```
env
```

![image](https://github.com/user-attachments/assets/c3e0b4ea-7201-4574-803d-48ab78f42a41)

要显示个别环境变量的值，可以使用 `printenv` 命令（不能使用 env 命令）；也可以`echo $var`。

![image](https://github.com/user-attachments/assets/b109b52d-04ee-4108-99e0-add6f3e89539)

## 局部变量
局部变量没有专门的命令，set命令可以查看全局+局部。

set 命令可以显示特定进程的所有环境变量，既包括局部变量、全局变量，也包括用户自定义变量。所有通过 env 命令或 printenv 命令能看到的全局环境变量都出现在了 set 命令的输出中。除此之外，还包括局部环境变量、用户自定义变量以及局部 shell 函数。

bash shell 的惯例是所有的环境变量均使用大写字母命名。如果是你自己创建或在 shell 脚 本中使用的局部变量，则使用小写字母命名。变量名区分大小写。坚持使用小写字母命 名用户自定义的局部变量，能够让你避免不小心与系统环境变量同名可能带来的灾难。

## export
export 命令的功能是将变量提升成环境变量，亦可将 shell 函数输出为环境变量。通常个人创建出的变量仅能在自己账户下使用，其他人是无法看到的。

![image](https://github.com/user-attachments/assets/fe4eb6af-8bf6-42b4-ac17-912a3559d145)

![image](https://github.com/user-attachments/assets/f0274043-c09d-42cb-8d7a-8002e6a837dd)

可以直接简化成：export my_var="I am out"

在定义并导出变量 my_variable 后，bash 命令生成了一个子 shell。在该子 shell 中可以正 确显示出全局环境变量 my_variable 的值。子 shell 随后改变了这个变量的值。但是，这种改变仅在子 shell 中有效，并不会反映到父 shell 环境中。

## unset
删除环境变量：unset 变量名

在涉及环境变量名时，什么时候该使用 $ ，什么时候不该使用 $ ，实在让人摸不着头脑。 只需记住一点：如果要用到（doing anything with）变量， 就使用 $ ；如果要操作（doing anything to） 变量，则不使用 $ 。这条规则的一个例外是使用 printenv 显示某个变量的值。

和修改变量一样，在子 shell 中删除全局变量后，无法将效果反映到父 shell 中。

## $PATH
PATH 环境变量定义了用于查找命令和程序的目录

如果命令或者程序所在的位置没有包括在 PATH 变量中，那么在不使用绝对路径的情况下， shell 是无法找到的。

修改PATH：
```
PATH=$PATH:/home/christine/Scripts
```
对于 PATH 变量的修改重启系统失效。

## 数组变量
有时候，数组变量只会把事情搞得更复杂，所以在 shell 脚本编程时并不常用。数组并不太方便移植到其他 shell 环境。

数组在shell中不常见。

![image](https://github.com/user-attachments/assets/f35c16d8-9ca6-4d88-83a4-052358a96cdf)

![image](https://github.com/user-attachments/assets/d5264f57-220a-4f32-9e9c-c5f129e277b1)

