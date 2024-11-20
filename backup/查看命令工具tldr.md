
## 项目主页与安装
开源主页：https://github.com/tldr-pages/tldr

安装方式：
```
pip3 install tldr
source .profile
tldr -u
```
使用：
```
tldr + 命令 
```

![image](https://github.com/user-attachments/assets/230ef904-3544-4f4c-8267-c48d1bbf1735)

## 使用注意
### 提示ssl错误
```
git config --global http.sslverify false
git config --global https.sslverify false
```
并确保网络是通畅的。

### 本地缓存设置
官方文档：

You can configure the behavior and output of the tldr client by setting environment variables. For example, in the .bashrc file:
```
export TLDR_COLOR_NAME="cyan"
export TLDR_COLOR_DESCRIPTION="white"
export TLDR_COLOR_EXAMPLE="green"
export TLDR_COLOR_COMMAND="red"
export TLDR_COLOR_PARAMETER="white"
export TLDR_LANGUAGE="es"
export TLDR_CACHE_ENABLED=1
export TLDR_CACHE_MAX_AGE=720
export TLDR_PAGES_SOURCE_LOCATION="https://raw.githubusercontent.com/tldr-pages/tldr/main/pages"
export TLDR_DOWNLOAD_CACHE_LOCATION="https://tldr-pages.github.io/assets/tldr.zip"
```

TLDR_CACHE_ENABLED (default is 1):

- If set to 1, the client will first try to load from cache, and fall back to fetching from the internet if the cache doesn't exist or is too old.
- If set to 0, the client will fetch from the internet, and fall back to the cache if the page cannot be fetched from the internet.

TLDR_CACHE_MAX_AGE (default is 168 hours, which is equivalent to a week): maximum age of the cache in hours to be considered as valid when TLDR_CACHE_ENABLED is set to 1.


Cache location

- In order of precedence:
- $XDG_CACHE_HOME/tldr
- $HOME/.cache/tldr
- ~/.cache/tldr

If you are experiencing issues with tldr, consider deleting the cache files before trying other measures.

改成自己的，比如：
```
export TLDR_CACHE_ENABLED=1
export TLDR_CACHE_MAX_AGE=7200
```
启用cache：
```
git clone https://gitcode.com/tldr-pages/tldr.git  ~/.cache/tldr
```
源码路径配置完事就能用了。