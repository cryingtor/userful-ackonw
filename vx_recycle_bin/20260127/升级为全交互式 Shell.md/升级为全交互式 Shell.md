# 升级为全交互式 Shell
1. 获取半交互式 Shell
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
script -qc /bin/bash /dev/null
```
使用Python、script 或 socat 方法之一获取基础交互环境。
2. 挂起当前 Shell
在客户端本地按 Ctrl + Z，暂时挂起当前 Shell，释放终端控制权。
3. 修改本地终端设置并恢复 Shell
执行：
```
stty raw -echo; fg
```
4.重置终端并配置环境
```
reset
export TERM=xterm-256color
export SHELL=/bin/bash
source /etc/skel/.bashrc  # 如果目标用户存在默认 bashrc 文件
```

