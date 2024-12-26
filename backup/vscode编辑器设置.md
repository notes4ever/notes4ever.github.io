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
# http://editorconfig.org
# vscode 安装 EditorConfig for VS Code 插件，该插件会自动读取项目的 .editorconfig 文件，对项目进行配置

root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为 utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 4 # 缩进大小
end_of_line = lf # 控制换行类型(lf | cr | crlf)
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false

# Tab indentation (no size specified)
[Makefile]
indent_style = tab
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
