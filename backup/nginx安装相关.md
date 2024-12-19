## linuxmint上使用apt安装
```
sudo apt update
sudo apt install nginx -y
```

安装完，默认nginx创建了www-data用户与用户组。

![image](https://github.com/user-attachments/assets/845fc20e-0534-4f08-a3ce-a749c42ba451)

此外，已经自动配置了开机自启动。

![image](https://github.com/user-attachments/assets/90d97f74-e6d2-44bc-b1ce-75dccd41a86f)

![image](https://github.com/user-attachments/assets/63e437a4-be44-4b58-a576-694df2dea024)

默认是一个守护进程（root用户启动），两个worker进程（www-data用户启动）。

常用命令：
```
sudo systemctl stop nginx
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
```

如果需要sudo + nginx命令不用输入密码，则需要编辑 /etc/sudoers 文件来配置:
```
nginxuser ALL=(ALL) NOPASSWD: /usr/sbin/nginx
```

nginxuser为普通用户。这样以后sudo + nginx命令就不用输入密码了。

此外，还有：
```
sudo nginx -s reload
sudo nginx -t 
```

![image](https://github.com/user-attachments/assets/bc2885c3-8147-424a-bc2a-58be5fa4de8c)


等等。

nginx的默认配置文件为：

![image](https://github.com/user-attachments/assets/3a8c6ad0-070d-4ee3-9e59-216df2cc215b)

设置普通用户编辑配置文件不用密码：
```
nginxuser ALL=(ALL) NOPASSWD: /bin/vim /etc/nginx/nginx.conf
```

或者修改文件属性，改成用户组www-data有修改配置的权限，然后将普通用户加入这个用户组：
```
sudo chown root:www-data /etc/nginx/nginx.conf
sudo chmod 0664 /etc/nginx/nginx.conf
```

![image](https://github.com/user-attachments/assets/00a7a216-8167-4e61-b8d9-4a5f3e2cb852)

普通用户加入www-data用户组：
```
sudo usermod -aG www-data nginxuser
```

![image](https://github.com/user-attachments/assets/4aca9965-5cfc-4765-a921-1218e8fc34f4)

以上操作完之后，需要重启nginx或者重启操作系统，才能最终生效，切记。

测试修改nginx配置文件，比如去掉nginx返回时候的版本信息。

![image](https://github.com/user-attachments/assets/2a6af639-fd4a-4b19-9447-2b319cfe9a80)

注释掉server_tokenks off;然后reload就生效了。

## 更好的安装方式
实际上更好地使用nginx，是不用apt来安装nginx，而且不用apt，也不用nginx，而是直接使用源码编译，而且是使用openresty来替换nginx，自由灵活，直接普通用户就可以，编译参数可选的很多。

## nginx一些命令
![image](https://github.com/user-attachments/assets/a91f1024-abf7-4506-a643-f3784a2857ae)
