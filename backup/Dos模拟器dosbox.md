官网：https://www.dosbox.com/

官网下载太慢，百度网盘：https://zhuanlan.zhihu.com/p/690971183

官方文档：https://www.dosbox.com/wiki

Mac相关：
- https://www.dosbox.com/wiki/DOSBox_and_Mac_OS_X
- https://www.dosbox.com/wiki/Mac_OSX_Advanced

使用教程参考：https://blog.csdn.net/YuzuruHanyu/article/details/80287419

里面有github，下载masm.exe, link.exe, debug.exe, exe2bin.exe后，放到~/dos文件夹，然后挂载到dosbox就完成环境搭建了。

高阶配置：自动挂载等。

```
[autoexec]
mount c ~/dos
path=%path%;\masm
c:
cd code
```

方案二：使用[dosbox-x](https://dosbox-x.com/),更先进一些。

代码仓库：https://gitee.com/mirrors/DOSBox-X

安装使用教程：https://blog.csdn.net/qq_41437512/article/details/127620587

实际上要考虑操作系统兼容性，比如Mac上，安装包打开显示已损坏，不兼容，这时候可能使用dosbox兼容性更好。
