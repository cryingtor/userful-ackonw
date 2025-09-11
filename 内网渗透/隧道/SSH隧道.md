# SSH隧道(无法C2上线,只做穿透)
# 正向:将远程主机的端口映射到本地，用于访问目标主机上的服务
<mark>ssh -L [本地IP:]本地端口:目标主机:目标端口 用户@跳板机</mark>
ssh -g -L 9999:localhost:22 -N kali@192.168.111.128

* -g	允许远程主机连接本地转发端口（默认只允许本地连接）

* -L 9999:localhost:22
* 本地端口转发：
本机监听 ​9999 端口​
连接到此端口的请求 → 发送到 kali@192.168.111.128
目标机将请求 → 转发给 ​目标机自身的 22 端口

* -N	不执行远程命令（只建立隧道）

kali@192.168.111.128	SSH 认证凭据（使用 kali 用户）

ssh -J user1@jump1,user2@jump2 user@final_target

访问跳板机后的内部SSH服务
ssh -p 9999 localhost

# 反向隧道:将本地服务暴露给远程主机，用于穿透防火墙/NAT
* 在被限制网络的主机上执行（连接到公有服务器）
ssh -fNT<mark>R</mark> 2222:localhost:22 kali@public-server.com
-R：远程端口转发（reverse forwarding）
2222:localhost:22：在公网服务器开启2222端口 → 转发到本机22端口

* 在公网服务器连接内网主机
ssh -p 2222 kali@localhost

暴露多个服务:
ssh -R 80:localhost:80 -R 443:localhost:443 -fN admin@cloud-server
# 跳板隧道
- 基本语法（SSH 7.3+）
ssh -J user1@jump1,user2@jump2 user@target

- 示例：通过两个跳板访问内网数据库
ssh -J admin@bastion,dba@db-gateway root@db-host

- 端口转发应用
ssh -J jump1,jump2 -L 3306:localhost:3306 user@db-host


# 第一跳：本地 -> 跳板1 (端口10001)
ssh -L 10001:target:22 user@jump1

# 第二跳：在跳板1上执行 -> 跳]`板2
ssh -L 10002:target:22 user@jump2 -p 10001

# 本地访问最终目标
ssh -p 10002 localhost
