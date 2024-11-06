![image](https://github.com/user-attachments/assets/2d792155-617e-4c79-9cef-298d8b91714e)

> **简洁，快速，稳定的Terminal**

## 忆往昔

记得多年前，第一次使用Linux（Ubuntu），打开终端，仿佛打开了一个新的世界。当时不断地折腾使用各种Linux发行版，各种安装和配置，这里面也包括Terminal。工作以后，一直在使用Linux Mint，已经陪伴了多年了。后来又跟Mac一起使用，Mac可以说是跟iPhone的精神内核类似，不能也不用折腾太多东西，简洁，稳定好用，为什么要去掉快速二字？因为我的Mac到了即将退休的年纪，现在简单的编译都要咆哮好久，最近在精简软件，并寻找更省资源的替代品。

## 又一款Terminal
之前在Mac上一直使用iTerm2，这款应该使用的时间最长，后来又换了Warp，配合zsh，都算是不错的体验，但都说不上完美，尤其是性能这个点，性能最好的替代品，[alacritty](https://github.com/alacritty/alacritty)应该就是暂时的答案了，56K的star属实有点惊人。Rust开发的，性能确实给了惊喜；期待这款Terminal的前景。

## 配置指导
简单花点时间，借鉴了别人的配置，也更换了图标，顺眼不少，参考如下：

- https://geekbb.xlog.page/Alacritty?locale=zh
- https://juejin.cn/post/7199193212777447484
- https://blog.csdn.net/yuoveyu/article/details/128388313

yml配置文件转换成toml，使用 alacritty migrate。
macos下如果找不到alacritty 命令，使用绝对路径：
/Applications/Alacritty.app/Contents/MacOS/alacritty migrate
然后删除yml文件，只保留toml即可。

## 更多搭配
还可以进一步搭配tmux，来弥补没有标签管理和session会话的劣势。