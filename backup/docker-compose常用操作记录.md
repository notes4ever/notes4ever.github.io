> **以elasticsearch skywalking安装过程作为示例。**

es官方文档：https://www.elastic.co/guide/en/elasticsearch/reference/8.11/docker.html

docker-compose官网的方式：
- https://docs.docker.com/compose/install/
- https://docs.docker.com/compose/install/standalone/

官网需要访问github，使用curl直接下载安装，也可以先下载好后使用。方式如下（Linux）：

https://github.com/docker/compose/releases/download/v2.23.3/docker-compose-linux-x86_64

下载下来后改名，然后放到/usr/local/bin/，赋予可执行权限。需要root权限。
```
cp  docker-compose-linux-x86_64 /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

安装以及配置用法参考：https://blog.csdn.net/qq_45868731/article/details/131743699

elasticsearch安装docker-compose方式，官网教程：

https://www.elastic.co/guide/en/elasticsearch/reference/8.11/docker.html#docker

里面搜compose有 .env与docker-compose.yml的配置样例。

elastic docker安装官方教程：

https://www.elastic.co/guide/en/elasticsearch/reference/8.11/docker.html

重点里面env 与 docker-compose配置文件：
- https://github.com/elastic/elasticsearch/tree/8.11/docs/reference/setup/install/docker
- https://github.com/elastic/elasticsearch/blob/8.11/docs/reference/setup/install/docker/docker-compose.yml

skywalking compose的配置文件，参考skywalking的docker目录，重点里面env 与 docker-compose配置文件：
- https://github.com/apache/skywalking/tree/master/docker
- https://github.com/apache/skywalking/blob/master/docker/docker-compose.yml

自己酌情修改env 与 docker-compose配置文件，多做测试。
