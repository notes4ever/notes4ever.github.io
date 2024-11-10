使用brew 安装：

`brew install llvm`

If you need to have llvm first in your PATH, run:

`echo 'export PATH="/usr/local/opt/llvm/bin:$PATH"' >> ~/.zshrc`

For compilers to find llvm you may need to set:

```
export LDFLAGS="-L/usr/local/opt/llvm/lib"
export CPPFLAGS="-I/usr/local/opt/llvm/include"
```