![image](https://github.com/user-attachments/assets/12192754-59a2-4a1e-bbb2-6a34ce4fbc83)

> **删除命令相关，rm权力太大，要限制，尤其是root和sudo执行rm。**

## 配置里直接echo来禁用
```
alias rm='echo " rm command is disabled."'
```

这种方式不好，就像是直接弹窗说，别用rm命令。

## 配置alias，引导到trash-cli
```
sudo apt install trash-cli
cd
vim .bashrc
alias rm='trash-put'
source .bashrc
```
**trash-cli常用命令：**
trash-list：列出回收站中清单
trash-restore：文件还原，需要在被删除的目录下使用，不然无效。
trash-empty：清空回收站（慎用或不用，除非空间不够）

**Linux系统中的回收站默认路径为$HOME/.local/share/Trash**

跟echo提示禁用一样，alias的方式，始终无法避免直接使用rm的绝对路径删除东西。

但这种方式比直接echo好一些，因为确实可以实现剪切，后面需要恢复的时候可以恢复；适用于自己平时使用rm命令，自己是可以控制不使用rm的绝对路径的，从而避免了误删除操作，需要真的删除的时候，定时进入回收站清空就可以，相当于cli版本的回收站，效果跟gui版本的回收站类似。

## 其他方式

- 如果使用jumpserver，可以直接禁用rm命令
- 使用脚本重定向rm的效果
