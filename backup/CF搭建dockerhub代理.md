> **cloudflare反向代理之dockerhub代理配置。**


项目主页：https://github.com/cmliu/CF-Workers-docker.io

参考教程：https://zhuanlan.zhihu.com/p/703901780

代理搭建完成之后，配置docker：

https://zhuanlan.zhihu.com/p/703693460

主要操作：

```
vim  /etc/docker/daemon.json
{
    "registry-mirrors":[
        "https://cf-workers-docker-io-pw4.pages.dev/"
    ]
}
```

重启docker：

```
# 重新加载 systemd 的配置
sudo systemctl daemon-reload

# 重启 Docker 服务
sudo systemctl restart docker
```

如果docker没开机启动可以配置开机自启：

sudo systemctl enable docker

查看docker源是否生效：

docker info

docker run hello-world