# nfs,path,cron提权

- cron(计划任务)
linux中计划任务会以root运行
```
1.寻找计划任务的地方
/etc/crontab
/etc/cron.d/ （检查该目录下所有文件）
/etc/cron.hourly/, /etc/cron.daily/, /etc/cron.weekly/, /etc/cron.monthly/ （检查目录和内部脚本）
/var/spool/cron/crontabs/ （用户级任务，通常需要 root 读取）
当前用户：crontab -l （可能权限很低）

2. ​检查关键文件和目录的权限 (最核心！)​
使用 ls -la 命令仔细检查权限位（首位 d 为目录）
全局可写 (World-Writable) 是最危险的标志！​​

​检查文件权限：​​
-rw-rw-rw- (666): 任何用户可读可写
-rwxrwxrwx (777): 任何用户可读可写可执行
-rw-rw---- (660) 或 -rwxrwx--- (770) 但文件所在组包含你的组且你有写权限：同样危险
​检查目录权限：​​
drwxrwxrwx (777): 任何用户可进入该目录（x），并创建、删除、重命名其中的文件（w）。
drwxrwxrwt (1777): Sticky bit (最后一个 t)，用户只能删除自己创建的文件。​不能阻止写入新文件或修改自己的文件！​​
drwxrwx--- (770) 且你的用户在该目录所属组：同样危险。


总结:
可利用 = (Cron任务以高权限(如root)运行) AND(非特权用户能控制该任务的执行内容)
```

- path(环境变量)
修改环境变量为suid程序路径,实现提权



- nfs(不需要考虑权限)
NFS（网络文件系统）**是一种分布式文件系统协议，允许计算机系统通过网络访问彼此的文件系统。
showmount -e x.x.x.x
mkdir nfs
chomd 777 shell.php(文件的权限会相同)

也可以配合suid提权,将find文件传入,同样会继承suid(若是不同版本的find如ubuntu,debain,会报错)
