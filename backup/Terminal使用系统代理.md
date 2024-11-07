> **除了浏览器使用系统代理，Terminal里面执行命令也可以使用，解决像brew和git等网络代理问题。**

## 配置proxy

export ALL_PROXY="socks5://127.0.0.1:2080"

或者更简洁的：

export ALL_PROXY=127.0.0.1:2080

去掉代理：

**export ALL_PROXY=""**

socks5h代表使用远程DNS而不是本地DNS，这在本地DNS混乱的情况下尤其有效。

## 配置git
vim  ~/.gitconfig

```
[user]
	email = xxx@xxx
	name = username
[filter "lfs"]
	clean = git-lfs clean -- %f
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
	required = true
[http]
        proxy = socks5h://127.0.0.1:2080
        sslVerify = false
```

sslVerify需要配置，不然可能会报错ssl相关。

## curl使用代理
![image](https://github.com/user-attachments/assets/cc552a8d-2915-4369-936a-c3b749b9fb75)

```
vim ~/.bashrc
export ALL_PROXY="socks5://127.0.0.1:2082"
source ~/.bashrc
curl --proxy $ALL_PROXY -I https://www.reddit.com
```
