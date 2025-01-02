![image](https://github.com/user-attachments/assets/1ce7a9f2-1914-4235-8aaa-bd83f587a67e)

miniconda官网：

https://docs.anaconda.com/miniconda/

参考教程：

https://zhuanlan.zhihu.com/p/685496400

## 主要安装命令

```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```

## conda使用示例
```
清除缓存：
conda clean --index-cache

更新conda本身：
conda update -n base -c defaults conda
conda update conda

使用示例：
conda create -n py310 python=3.10
conda activate py310
conda deactivate

移除环境 如果没有"--all",会出现移除不干净的情况:
conda remove -n "envName" --all 

查看所有环境：
conda info -e
conda info --envs

查看当前环境已安装的包列表：
conda list

查看conda、环境相关信息:
conda info
```

## conda中使用jupyterlab
官网：https://jupyter.org/install 

安装:
```
pip install jupyterlab
```

启动：
```
jupyter lab
```

参考：https://www.jianshu.com/p/c0275f9113e3