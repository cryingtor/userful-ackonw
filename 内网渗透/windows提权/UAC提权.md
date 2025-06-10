# UAC提权
UAC:就是点击运行程序时出现的对话框
为了远程执行exe等,需绕过此安全机    制
UAC的开启会对msf的getsystem功能有影响
绕过:MSF内置模块,Powershell,UACME项目


- MSF模块
use exploit/windows/loacal/bupassua(ask,bypassuac_sluihijack,bypassuac_silentcleanup)
- UACME项目
AKagi


