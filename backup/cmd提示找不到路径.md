windows上打开cmd窗口时提示找不到路径，可能是注册表里面的autorun导致。

注册表:

```
HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor 
HKEY_CURRENT_USER\Software\Microsoft\Command Processor
```

解决方法：要么修改，要么删除autorun里面的参数值。