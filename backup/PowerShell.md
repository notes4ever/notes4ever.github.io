## 查看PowerShell版本
在PowerShell中执行：
```
$PSVersionTable.PSVersion
```

![image](https://github.com/user-attachments/assets/03737299-c1dc-4f7f-b4aa-4135ebbe2cbb)

## 搜索最新版本

```
winget search Microsoft.PowerShell
```

![image](https://github.com/user-attachments/assets/abfc93ac-394f-4ad6-a505-ef09bf08e544)

## 安装新版本
使用 Winget 安装 PowerShell:
```
winget install --id Microsoft.Powershell --source winget
```

使用 Winget 安装 PowerShell 预览版
```
winget install --id Microsoft.Powershell.Preview --source winget
```

## 禁止启动时检查版本
启动的命令添加 –nologo。

![image](https://github.com/user-attachments/assets/c7a53f12-616b-4409-b110-fede2bb1712e)

