# Collpg
nmap扫描
```
└─$ nmap -sV -T4 -sC -p- 192.168.56.116
Starting Nmap 7.95 ( https://nmap.org ) at 2026-02-14 15:59 CST
Nmap scan report for 192.168.56.116
Host is up (0.0012s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 10.0p2 Debian 7 (protocol 2.0)
80/tcp open  http    nginx
|_http-title: CoolPG Internal
MAC Address: 08:00:27:81:5E:CB (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.00 seconds
```
![](vx_images/300003656979384.png)
是一个登录界面
继续做一下信息收集,扫描目录
目录没扫出来
翻看网页源码发现
![](vx_images/161654300376593.png)
提示入职：向IT申请访问权限
在尝试将it拼接目录访问时url将会跳转coolpgi.hmv
这里在host文件中修该域名指向coolpgi.hmv,尝试爆破一下域名(没有结果)
重新在词扫描目录发现/search,panel路径
![](vx_images/86175747532958.png)
查询到admin用户存在
尝试爆破一下密码
没爆破出来
提示:
![](vx_images/224869568579143.png)
尝试直接sqlmap跑
![](vx_images/589904363359647.png)
![](vx_images/379673665581333.png)
ssh链接
![](vx_images/11341881013707.png)
提权:
![](vx_images/503662737507461.png)
直接拿到
![](vx_images/236444027415280.png)


