# nps

## NPS 服务端​

​部署位置​：必须安装在一台具有公网 IP 地址的服务器上（如云服务器：阿里云、腾讯云等）。

​角色​：作为控制中心和流量中转站。

​功能​：管理客户端，验证连接，接收外部请求并将其转发到正确的内网客户端。

## ​NPS 客户端​

​部署位置​：安装在你需要穿透的内网机器上（如家里的 NAS、办公电脑、本地开发的 Web 服务器）。

​角色​：与服务端建立稳定连接，并注册自己需要暴露的服务。

​别名​：也叫 ​NPC。

```
编辑 conf/nps.conf文件。关键配置项



# 服务端的管理端口（用于Web管理界面）
web_port = 8080
# 服务端与客户端通信的端口
bridge_port = 8024
# 管理界面的用户名和密码
web_username = admin
web_password = 123
# 如果你需要穿透 HTTPS 服务，需要配置域名和 SSL 证书目录
# https_proxy_ip=0.0.0.0
# https_proxy_port=443
# https_default_cert_file=conf/server.pem
# https_default_key_file=conf/server.key
```

启动:
```
# 直接启动（前台运行）
sudo ./nps
# 或，安装为系统服务（推荐，开机自启）
sudo ./nps install
sudo nps start
```