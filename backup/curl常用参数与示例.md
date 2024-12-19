## -I 参数
使用：curl -I url
http响应的头部信息（header）不需要知道返回内容body的时候使用大写-I参数

## -i 参数
使用：curl -i  url
显示响应头、返回内容body

## -v 参数
-v：显示最详细的信息
包含请求头、响应头、返回内容

## -o 参数
curl -o：保存为文件

## -L 参数
curl -L
重定向301，追踪重定向

## -D 参数
curl  url  -D   存放的文件名
将响应头存输出到文件中。-D可以放在url前面或后面都行

## -X 参数
curl -X  (POST、GET、PUT、PATCH、DELETE、HEAD)  url 
-X指定请求方法

## -H 参数
curl -H "name: value"
自定义请求头，只是请求，服务器并不一定返回。
curl -H "name:" 
参数名后面留空，表示移除这个请求头

## curl 结合代理使用
curl 结合socks http代理使用：
```
curl -I --socks5-hostname localhost:10808 https://www.google.com/
curl -I  -socks5-hostname localhost:10808   https://www.apple.com/library/test/success.html 
curl -I socks5h://localhost:10808   https://www.google.com/
```
报错：
Protocol "socks5h" not supported  要先支持新版本才行。