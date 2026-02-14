# 交互式shell终端
```
script /dev/null -c bash  # 创建伪终端会话
Ctrl+Z                    # 挂起当前会话
stty raw -echo; fg        # 设置终端原始模式并恢复会话
reset xterm               # 重置终端设置
export TERM=xterm-256color         # 设置终端类型
export SHELL=/bin/bash               # 查看当前 Shell
stty rows 59 cols 236     # 设置终端行列数
source /etc/skel/.bashrc
```