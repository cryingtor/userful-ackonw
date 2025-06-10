# ctfshow-web4
首先测试伪协议,测试后发现本题对伪协议做了限制,关键字符受到了过滤,尝试大小写,双写,无法绕过,考虑日志文件包含nginx
文件路径:/var/log/nignx/access.log
传入参数/?url=/var/log/nignx/access.log
这里进行抓包修改UA头,或者用Hackerbar

![](vx_images/536695045404678.png =814x)

成功后用蚁剑连接(用http://连接)翻看目录看到flag
![](vx_images/88574791690720.png =819x)