SSH终端登录时默认使用的是**非交互式的登录方式**（non-interactive login），这可能导致无法自动执行source ~/.bashrc。

如果使用的是SSH密钥进行登录，可以尝试在登录命令中添加-i参数以使用交互式登录方式（interactive login），例如：
```
ssh -i /path/to/private_key user@host
```

这样可以确保在登录时自动执行source ~/.bashrc。