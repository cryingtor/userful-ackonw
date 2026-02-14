# react
起手nmap
![](vx_images/491045213848503.png)
80端口是一个网络诊断
![](vx_images/250127544876000.png)
3000端口是react框架
![](vx_images/49167127807799.png)
进行一下目录扫描
只扫出一个phpinfo页面
![](vx_images/28359428548971.png)
试一下前段时间爆出的react的rce漏洞
![](vx_images/255529132286682.png)
确实存在
找个可以利用的poc
https://github.com/zack0x01/CVE-2025-55182-advanced-scanner-/blob/main/scanner.sh
随后用nc弹回shell
![](vx_images/591248404141304.png)
先升级一下shell的交互
![](vx_images/124338948351087.png)
在用户目录下找到flag

提权root:
查看可以使用的特权命令
![](vx_images/244345547812585.png)
scanner.py也是一个react的poc脚本,尝试以root身份运行
这里直接使用sudo /opt/react2shell/scanner.py命令
但是这里无论干什么都无法扫描出漏洞,刷新web页面也一直卡住,一直超时
看了下方法通过写公钥解决
![](vx_images/563655734217399.png)
![](vx_images/516029367147495.png)
使用命令sudo /opt/react2shell/scanner.py -l /root/root.txt会从中读取列表,但是会将文件中的内容报错显示出来,导致了一个特权文件披露漏洞(root.txt被我搞没了用user.txt示例)
![](vx_images/187544301104681.png)





