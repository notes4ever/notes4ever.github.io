## 方式一，通过finder修改
可以参考[这篇](https://blog.csdn.net/hhi00/article/details/137923625)进行尝试。

## 方式二，通过chmod修改
这是stream++ 官网建议的修改方式：
```
sudo chmod +a 'user:molock:allow read' /etc/hosts
sudo chmod +a 'user:molock:allow write' /etc/hosts
```
这种貌似有问题，临时可以，过一会又不行了。

## 方式三，通过chflags命令修改
```
sudo chflags nouchg /etc/hosts
sudo chflags noschg /etc/hosts
sudo chflags -hv noschg /etc/hosts
```

