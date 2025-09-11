# qouted
nmap扫描
![](vx_images/314486252982808.png)
存在ftp匿名登陆,好像是web的文件
![](vx_images/260852787708950.png)
web是一个iis的页面,通过ftp尝试上传asp后门
![](vx_images/231592476644505.png)
确保能够访问
![](vx_images/587162078972440.png)
使用哥斯拉成功连接
![](vx_images/400062451080454.png)
上线CS
![](vx_images/162474167269536.png)
使用sweetpotato提权
![](vx_images/357908460579237.png)
在quoted用户desktop下找到uesrflag
![](vx_images/339658110929158.png)
HMV{User_Flag_Obtained}
在administrator用户desktop下找到flag
![](vx_images/235909970601585.png)
HMV{Elevated_Shell_Again}

看一下其他思路;
1.dll劫持
2.juicypotato
3.msf本地漏洞探测

