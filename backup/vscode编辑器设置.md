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

## 重要参数配置
code-runner.runInTerminal  参数：

开启则在vscode终端执行，windows下不需要输入参数的话，可以关闭；然后就会在输出界面输出结果。

 vscode输出界面每次运行时自动清屏后再输出，这样每次都能定位到最新的输出（最后一行）  ，配置参数：

![image](https://github.com/user-attachments/assets/dc8c4348-2ccd-4c9a-9c9e-04d384b73855)

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

## settings.json - Windows
```
{
    // 主题
    "workbench.colorTheme": "GitHub Dark",

    // 关于缩进 tab 空格
    // 检测第一个tab后面就tab 设置为false禁止这种智能判断
    "editor.detectIndentation": false,
    "editor.tabSize": 4,
    "editor.insertSpaces": true,    // 按 Tab 键时插入空格
    "editor.renderControlCharacters": true, //制表符显示->
    "editor.renderWhitespace": "selection", // 显示空格 tab 设置selection或者all

    // 字体设置
    "editor.fontFamily": "JetBrains Mono,HONOR Sans Cn,Consolas,monospace",
    "terminal.integrated.fontFamily": "JetBrains Mono,monospace",
    "terminal.integrated.fontWeight": "normal",
    "editor.fontSize": 16,
    "terminal.integrated.fontSize": 14,
    "editor.fontLigatures": false,
    "editor.fontVariations": false,
    // 注释字体设置为斜体
    "editor.tokenColorCustomizations": {
        "textMateRules": [{
            "scope": "comment",
            "settings": {
            "fontStyle": "italic"
        }}],
    },

    // 换行与换行竖线 设置80字符
    "editor.rulers": [80],
    "editor.wordWrap": "wordWrapColumn",
    "editor.wordWrapColumn": 80,

    // 文件编码与自动保存
    "files.autoGuessEncoding": true,
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 1000,

    // 格式化相关
    "editor.defaultFormatter": "xaver.clang-format",
    "editor.formatOnSave": true,
    "editor.formatOnSaveMode": "file",
    "editor.formatOnPaste": true,
    "files.trimTrailingWhitespace": true,

    // 右边小地图
    "editor.minimap.enabled": true,

    // 语言相关的配置
    "C_Cpp.default.cppStandard": "c++17",
    "C_Cpp.default.cStandard": "c17",
    "C_Cpp.formatting": "clangFormat",
    "[c]": {
        "editor.defaultFormatter": "xaver.clang-format"
    },
    "[python]": {
        "editor.formatOnType": true
    },
    "python.defaultInterpreterPath": "C:\\miniconda3\\python.exe",

    // 插件配置 code-runner
    "code-runner.executorMap": {
        "c": "cd $dir && gcc -std=c17 -g -Wall $fileName -o D:\\TMP\\BUILD\\$fileNameWithoutExt.exe && D:\\TMP\\BUILD\\$fileNameWithoutExt.exe",
        "cpp": "cd $dir && g++ -std=c++17 -g -Wall $fileName -o D:\\TMP\\BUILD\\$fileNameWithoutExt.exe && D:\\TMP\\BUILD\\$fileNameWithoutExt.exe"
    },
    "code-runner.saveFileBeforeRun": true,
    "code-runner.fileDirectoryAsCwd": true,
    "code-runner.saveAllFilesBeforeRun": true,
    "code-runner.runInTerminal": true,
    "code-runner.clearPreviousOutput": true,
    "debug.terminal.clearBeforeReusing": true,

    // 注释插件 doxygen 配置
    "doxdocgen.file.copyrightTag": [
      "@copyright Copyright (c) {year}"
    ],
    "doxdocgen.file.fileTemplate": "@file {name}",
    "doxdocgen.file.versionTag": "@version 1.0",
    "doxdocgen.generic.authorEmail": "yianny@163.com",
    "doxdocgen.generic.authorName": "yianny",
    "doxdocgen.generic.authorTag": "@author {author} ({email})",
    "doxdocgen.generic.order": [
        "brief",
        "tparam",
        "param",
        "return"
    ],
    "doxdocgen.generic.paramTemplate": "@param {param}",
    "doxdocgen.generic.returnTemplate": "@return {type}",
    "doxdocgen.generic.splitCasingSmartText": true,
    "doxdocgen.file.fileOrder": [
        "file",
        "brief",
        "version",
        "date",
        "author",
        "copyright",
    ],

    // 解决windows下 部分场景下的终端乱码
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

    // 当前行的高亮设置
    // "workbench.colorCustomizations": {
    //     "editor.lineHighlightBackground": "#00000000",
    //     "editor.lineHighlightBorder": "#00000000"
    // },

    // 其他
    "files.associations": {
    },
    "files.exclude": {
        "**/*.exe": true,
        "**/build": true,
        "**/.git": true,
        "**/node_modules": true,
        "**/.DS_Store": true,
        "**/dist": true,
    },
    "C_Cpp.files.exclude": {
        "**/.vscode": true,
    },

    "editor.accessibilitySupport": "off",
    "extensions.ignoreRecommendations": true,
    "workbench.startupEditor": "none",
    "explorer.confirmDelete": false,
    "output.smartScroll.enabled": false,
    "Codegeex.Comment.LanguagePreference": "中文",
    "Codegeex.Chat.LanguagePreference": "中文",
    "Codegeex.SidebarUI.LanguagePreference": "中文",
    "Codegeex.Explanation.LanguagePreference": "zh-CN",
    "security.workspace.trust.untrustedFiles": "open",
    "security.workspace.trust.startupPrompt": "never",
    "Codegeex.Privacy": true,

}
```

## settings.json - Mac
```
{
    "workbench.colorTheme": "GitHub Light",
    // 关于缩进 tab 空格
    // 检测第一个tab后面就tab 设置为false禁止这种智能判断
    "editor.detectIndentation": false,
    "editor.tabSize": 4,
    "editor.insertSpaces": true, // 按 Tab 键时插入空格
    "editor.renderControlCharacters": true, //制表符显示->
    "editor.renderWhitespace": "selection", // 显示空格 tab 设置selection或者all
    // 字体设置
    "editor.fontFamily": "Iosevka,JetBrains Mono,Menlo,HONOR Sans Cn,Consolas,monospace",
    "terminal.integrated.fontFamily": "JetBrains Mono,monospace",
    "terminal.integrated.fontWeight": "normal",
    "editor.fontSize": 13.5,
    "terminal.integrated.fontSize": 11,
    "editor.fontLigatures": false,
    "editor.fontVariations": false,
    // 注释字体设置为斜体
    "editor.tokenColorCustomizations": {
        "textMateRules": [
            {
                "scope": "comment",
                "settings": {
                    "fontStyle": "italic"
                }
            }
        ],
    },
    // 换行与换行竖线 设置80字符
    "editor.rulers": [
        80
    ],
    "editor.wordWrap": "wordWrapColumn",
    "editor.wordWrapColumn": 80,
    // 文件编码与自动保存
    "files.autoGuessEncoding": true,
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 1000,
    // 格式化相关
    "editor.defaultFormatter": "xaver.clang-format",
    "editor.formatOnSave": true,
    "editor.formatOnSaveMode": "file",
    "editor.formatOnPaste": true,
    "files.trimTrailingWhitespace": true,
    // 右边小地图
    "editor.minimap.enabled": true,
    // 语言相关的配置
    "C_Cpp.default.cppStandard": "c++17",
    "C_Cpp.default.cStandard": "c17",
    "C_Cpp.formatting": "clangFormat",
    "[c]": {
        "editor.defaultFormatter": "xaver.clang-format"
    },
    "[python]": {
        "editor.formatOnType": true
    },
    "python.defaultInterpreterPath": "/usr/local/Caskroom/miniconda/base/bin/python",
    "python.condaPath": "/usr/local/Caskroom/miniconda/base/bin/conda",
    // 插件配置 code-runner
    "code-runner.executorMap": {
        "c": "cd $dir && gcc -std=c17 -g -Wall $fileName -o ~/build/$fileNameWithoutExt_c && ~/build/$fileNameWithoutExt_c",
        "cpp": "cd $dir && g++ -std=c++17 -g -Wall $fileName -o ~/build/$fileNameWithoutExt_cpp && ~/build/$fileNameWithoutExt_cpp"
    },
    "code-runner.saveFileBeforeRun": true,
    "code-runner.fileDirectoryAsCwd": true,
    "code-runner.saveAllFilesBeforeRun": true,
    "code-runner.runInTerminal": true,
    "code-runner.clearPreviousOutput": true,
    "debug.terminal.clearBeforeReusing": true,
    // 注释插件 doxygen 配置
    "doxdocgen.file.copyrightTag": [
        "@copyright Copyright (c) {year}"
    ],
    "doxdocgen.file.fileTemplate": "@file {name}",
    "doxdocgen.file.versionTag": "@version 1.0",
    "doxdocgen.generic.authorEmail": "yianny@163.com",
    "doxdocgen.generic.authorName": "yianny",
    "doxdocgen.generic.authorTag": "@author {author} ({email})",
    "doxdocgen.generic.order": [
        "brief",
        "tparam",
        "param",
        "return"
    ],
    "doxdocgen.generic.paramTemplate": "@param {param}",
    "doxdocgen.generic.returnTemplate": "@return {type}",
    "doxdocgen.generic.splitCasingSmartText": true,
    "doxdocgen.file.fileOrder": [
        "file",
        "brief",
        "version",
        "date",
        "author",
        "copyright",
    ],
    // 解决windows下 部分场景下的终端乱码
    "terminal.integrated.profiles.windows": {
        "PowerShell -NoProfile": {
            "source": "PowerShell",
            "args": [
                "-NoProfile",
                "-NoExit",
                "-Command",
                "chcp 65001"
            ]
        }
    },
    "terminal.integrated.defaultProfile.windows": "PowerShell -NoProfile",
    // 当前行的高亮设置
    // "workbench.colorCustomizations": {
    //     "editor.lineHighlightBackground": "#00000000",
    //     "editor.lineHighlightBorder": "#00000000"
    // },
    // 其他
    "files.associations": {},
    "files.exclude": {
        "**/*.exe": true,
        "**/build": true,
        "**/.git": true,
        "**/node_modules": true,
        "**/.DS_Store": true,
        "**/dist": true,
    },
    "C_Cpp.files.exclude": {
        "**/.vscode": true,
    },
    "editor.accessibilitySupport": "off",
    "extensions.ignoreRecommendations": true,
    "workbench.startupEditor": "none",
    "explorer.confirmDelete": false,
    "output.smartScroll.enabled": false,
    "Codegeex.Comment.LanguagePreference": "中文",
    "Codegeex.Chat.LanguagePreference": "中文",
    "Codegeex.SidebarUI.LanguagePreference": "中文",
    "Codegeex.Explanation.LanguagePreference": "zh-CN",
    "security.workspace.trust.untrustedFiles": "open",
    "security.workspace.trust.startupPrompt": "never",
    "Codegeex.Privacy": true,
}
```

## c_cpp_properties.json - Mac
```
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/local/Cellar/gcc/14.1.0_2/lib/gcc/current/gcc/x86_64-apple-darwin21/14/include",
                "/usr/local/Cellar/gcc/14.1.0_2/lib/gcc/current/gcc/x86_64-apple-darwin21/14/finclude",
                "/usr/local/Cellar/gcc/14.1.0_2/lib/gcc/current/gcc/x86_64-apple-darwin21/14/include-fixed",
                "/usr/local/Cellar/gcc/14.1.0_2/include/c++/14",
                "/usr/local/Cellar/gcc/14.1.0_2/lib/gcc/current/gcc/x86_64-apple-darwin21/14",
                "/usr/local/Cellar/gcc/14.1.0_2"
            ],
            "defines": [],
            "macFrameworkPath": [],
            "compilerPath": "/usr/local/Cellar/gcc/14.1.0_2/bin/gcc-14",
            "intelliSenseMode": "macos-gcc-x64",
            "cStandard": "c17",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}
```

## task.json - Windows
```
{
  "tasks": [
    {
      "type": "cppbuild",
      "label": "C/C++: g++.exe 生成活动文件",
      "command": "D:\\Dev-Cpp\\MinGW64\\bin\\g++.exe",
      "args": [
        "-fdiagnostics-color=always",
        "-g",
        "${file}",
        "-o",
        "${fileDirname}\\${fileBasenameNoExtension}.exe"
      ],
      "options": {
        "cwd": "${fileDirname}"
      },
      "problemMatcher": ["$gcc"],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "detail": "调试器生成的任务。"
    }
  ],
  "version": "2.0.0"
}
```

## launch.json - Windows
```
{
  "configurations": [
    {
      "name": "C/C++: g++.exe 生成和调试活动文件",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${fileDirname}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "D:\\Dev-Cpp\\MinGW64\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "为 gdb 启用整齐打印",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        },
        {
          "description": "将反汇编风格设置为 Intel",
          "text": "-gdb-set disassembly-flavor intel",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "C/C++: g++.exe 生成活动文件"
    }
  ],
  "version": "2.0.0"
}
```