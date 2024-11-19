`curl -vk https://www.baidu.com`

`openssl s_client -tlsextdebug -showcerts -connect www.baidu.com:443`

用strace跟踪：

`strace openssl s_client -tlsextdebug -showcerts -connect www.baidu.com:443`

在需要分析 OpenSSL 为什么报错的时候，你可以在前面加上 strace，这对于排查根因有不少的帮助。

注意：openssl 输出结果非常多。

在 Wireshark 里导出 Cipher Suite 的方法：

在 TLS 详情中选中 Cipher Suite，右单击，选中 Copy，在次级菜单中选中 All Visible Selected Tree Items。这时，列表就被复制出来了。

除此之外，我们还在排查 TLS Alert 40 这个信息时，通过查阅RFC5246得到了答案。
所以，在遇到一些协议类型、定义相关的问题时，最好查阅权威的 RFC 文档，这样可以获得最准确的信息。