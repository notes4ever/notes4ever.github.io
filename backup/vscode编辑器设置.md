## 文件编码自动检测
![image](https://github.com/user-attachments/assets/6255a4c3-2dc8-4950-a0d0-fa299221c25e)

## 终端UTF-8 配置
参考：https://zhuanlan.zhihu.com/p/688941159

实际采用：setting.json的方式。
```
"terminal.integrated.profiles.windows": {
    "PowerShell -NoProfile": {
      "source": "PowerShell",
      "args": ["-NoProfile",
        "-NoExit",
        "-Command",
        "chcp 65001"
       ]
    }
  },
  "terminal.integrated.defaultProfile.windows": "PowerShell -NoProfile",
```

## .editorconfig文件使用示例
使用示例：
```
# .editorconfig
[*.py]
indent_style = space
indent_size = 4
max_line_length = 100
```

## settings.json文件使用示例
```
{
    "[python]": {
        "editor.defaultFormatter": "ms-python.autopep8",
        "editor.formatOnSave": true
    },
    "code-runner.runInTerminal": false,
    "python.pythonPath": "C:\\miniconda3\\python.exe",
    "code-runner.executorMap": {
        "python": "set PYTHONIOENCODING=utf8 & $pythonPath $fullFileName",
    },
    "editor.fontSize": 12,
    "editor.fontFamily": "Hack,Fira Code,HONOR Sans Cn,Consolas,monospace",
    "terminal.integrated.defaultProfile.windows": "PowerShell",
}
```
## 重要参数配置
code-runner.runInTerminal  参数：

开启则在vscode终端执行，windows下不需要输入参数的话，可以关闭；然后就会在输出界面输出结果。

 vscode输出界面每次运行时自动清屏后再输出，这样每次都能定位到最新的输出（最后一行）  ，配置参数：

![image](https://github.com/user-attachments/assets/dc8c4348-2ccd-4c9a-9c9e-04d384b73855)
