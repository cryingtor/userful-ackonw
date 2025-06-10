# web服务器(win7)进程注入与提权
我们的shell为32位,注入一个64位的进程,获得64位的shell
shell tasklist列出进程,
![](vx_images/29984985461829.png)
选择目标进程(优先选择非系统进程,防止系统崩溃)
这里选择
![](vx_images/14564731375474.png)
pid:2896
先配置新的监听器
![](vx_images/465814181406208.png)

执行命令
inject 2896 x64
选择新的监听器
![](vx_images/201884375631659.png)
成功注入:
![](vx_images/514034127369346.png)
用64位的shell来提权,
由于为adminstrator,直接输入getsystem获得system用户权限
![](vx_images/505196219732456.png)
成功完全控制web服务器
用CS自带的mimikatz提取本地密码,哈希函数读取
![](vx_images/535803975033766.png)