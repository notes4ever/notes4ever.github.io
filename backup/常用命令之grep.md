![image](https://github.com/user-attachments/assets/3d7aa7e7-1334-4d89-b06e-6bee02bec18f)

## grep

grep 命令来自英文词组 global search regular expression and print out the line 的缩写，意思 是用于全面搜索的正则表达式，并将结果输出。人们通常会将 grep 命令与正则表达式搭配使 用，参数作为搜索过程中的补充或对输出结果的筛选，命令模式十分灵活。

![image](https://github.com/user-attachments/assets/102f2e4d-98cf-4e2e-9b9c-96401014a991)

![image](https://github.com/user-attachments/assets/10d69a68-a472-49b5-9dec-5ba69b8ee9a6)

![image](https://github.com/user-attachments/assets/5155ae12-4c27-461a-9286-3465bb5f8450)

## egrep 与 fgrep
与grep容易混淆的是 egrep 命令和 fgrep 命令。 

如果把 grep 命令当作标准搜索命令， 那么 **egrep 则是扩展搜索命令， 等价于 grep -E 命令， 支持扩展的正则表达式**。

 而 fgrep 则是**快速搜索命令，等价于 grep -F 命令，不支持正则表达式，直接按照字符串内容进行匹配**。