# nfs,path,cron提权

- cron(计划任务)
linux中计划任务会以root运行


- path(环境变量)



- nfs(不需要考虑权限)
NFS（网络文件系统）**是一种分布式文件系统协议，允许计算机系统通过网络访问彼此的文件系统。
showmount -e x.x.x.x
mkdir nfs
chomd 777 shell.php(文件的权限会相同)

也可以配合suid提权,将find文件传入,同样会继承suid(若是不同版本的find如ubuntu,debain,会报错)
