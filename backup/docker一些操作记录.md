参考：
- [Docker快速入门总结笔记](https://blog.csdn.net/huangjhai/article/details/118854733)

记录一些相对常用的docker操作命令。

## docker显示完整命令
```
docker ps -a --no-trunc
```

## 卸载旧的docker版本 
```
yum remove docker \ 
 docker-client \ 
 docker-client-latest \ 
 docker-common \ 
 docker-latest \ 
 docker-latest-logrotate \ 
 docker-logrotate \ 
 docker-engine 
```

## 主要安装步骤
设置docker镜像仓库：

```
yum install -y yum-utils 

yum-config-manager \ 
 --add-repo \ 
 https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo 
```

安装：
```
yum makecache fast 

yum install docker-ce docker-ce-cli containerd.io 
```

启动并验证：

```
systemctl start docker 
systemctl enable docker 
docker run hello-world 
docker images 
```

## 卸载刚装的版本
```
yum remove docker-ce docker-ce-cli containerd.io 
rm -rf /var/lib/docker 
```

## 配置阿里云镜像加速
```
sudo mkdir -p /etc/docker 

sudo tee /etc/docker/daemon.json <<-'EOF' 
{ 
 "registry-mirrors": ["https://axvfsf7e.mirror.aliyuncs.com"] 
} 
EOF 

sudo systemctl daemon-reload 
sudo systemctl restart docker 
```

## 普通用户授权
```
# 为用户新增添加一个附加组docker 
sudo usermod –aG docker $USER 

普通用户验证： 
docker version 
docker run hello-world 
```

## docker常用参数
```
docker search mysql --filter=STARS=3000 

docker pull mysql5.7 

docker rmi 删除镜像
 
docker rmi -f 镜像id 

docker rmi -f $(docker images -aq) 删除全部 

docker run -it centos /bin/bash 进入容器 

docker logs -tf 容器id 

docker logs --tail number 容器id #num为要显示的日志条数 

查看容器的元数据 :
docker inspect 容器id 

拷贝容器的文件到主机中:
docker cp 容器id:容器内路径 目的主机路径 

拷贝宿主机的文件到容器中 :
docker cp 目的主机路径 容器id:容器内路径 

docker exec -it c703b5b1911f /bin/bash 
```

## docker run 示例
基本用法：
```
docker run -d --name nginx01 -p 3334:80 nginx 
-d 后台运行 
--name 给容器命名 
-p 3334:80 将宿主机的端口3334映射到该容器的80端口 
```

进阶用法，以jenkins为示例：
```
docker run --rm -d --name jenkins-docker \ 
 -p 8080:8080 -p 50000:50000 \ 
 -v /home/ht/ht-devops/jenkins/jenkins_home:/var/jenkins_home \ 
 jenkinsci/blueocean 

docker run -d --rm --name jenkins-docker \ 
 -p 8080:8080 -p 50000:50000 \ 
 -v jenkins_home:/var/jenkins_home \ 
 jenkinsci/blueocean 
 挂载目录如果是相对路径，比如上面，则会创建/var/lib/docker/volumes/jenkins-data 
```

探索docker在非root用户情形下的使用情况 :
```
docker run -u root -v /var/jenkins/data:/var/jenkins_home -p 8080:8080 jenkinszh/blueocean-zh:2.204.5 

docker run -d --name jenkins -p 8080:8080 -v /home/ht/ht-devops/jenkins/jenkins_home:/var/jenkins_home jenkinszh/blueocean-zh:2.204.5 

su - root

chown -R 1000:1000 /home/ht/ht-devops/jenkins/jenkins_home 

备注： 

-d //启动在后台 
--name //容器名字 
-p //端口映射（8081：宿主主机端口，8080：容器内部端口） 
-v //数据卷挂载映射（/data/jenkins_home：宿主主机目录，另外一个即是容器目录） 
jenkins/jenkins:lts //Jenkins镜像（最新版） 

docker run --rm --name jenkins-docker -d -p 8080:8080 -p 50000:50000 jenkinszh/blueocean-zh:2.204.5 
898833f858b24061b49301dbe401723d 
This may also be found at: /var/jenkins_home/secrets/initialAdminPassword 

jenkins blueocean容器hub： 
https://hub.docker.com/r/jenkinsci/blueocean 
https://www.jenkins.io/doc/book/blueocean/ 
```

一些参考：
- [手把手教你使用 Jenkins+Docker 实现持续集成](https://zhuanlan.zhihu.com/p/89312003)
- [谈谈 Docker Volume 之权限管理](https://developer.aliyun.com/article/53990)