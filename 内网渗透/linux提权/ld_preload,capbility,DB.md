# ld_preload,capbility,DB

- DB
udf提权

- ld_preload


- capbility
文章:
[16026088.html](https://www.cnblogs.com/f-carey/p/16026088.html)

设置能力:setcap cap_setuid+ep /tmp/php
删除能力:setcap -r /tmp/php
查看单个能力:getcap /user/bin/php
查看所有能力:getcap -r / 2>/dev/null
结论:suid的升级版,有更细致的权限划分,通过查看有哪些权限来做利用