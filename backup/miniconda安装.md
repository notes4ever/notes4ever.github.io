![image](https://github.com/user-attachments/assets/1ce7a9f2-1914-4235-8aaa-bd83f587a67e)

miniconda官网：

https://docs.anaconda.com/miniconda/

参考教程：

https://zhuanlan.zhihu.com/p/685496400

主要命令：

```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```
