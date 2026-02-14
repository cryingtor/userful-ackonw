# socat(可转发端口)
socat TCP-LISTEN:3031,fork TCP:127.0.0.1:3002
```
TCP-LISTEN:3031- 在 3031 端口上监听 TCP 连接
fork- 重要参数，允许多个并发连接
TCP:127.0.0.1:3002- 将所有连接转发到本地 3002 端口
```