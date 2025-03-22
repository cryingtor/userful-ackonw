# 基础代理配置
SOCKSPort 9050
Log notice stdout
DataDirectory D:\Tor_Expert\data

# 启用网桥模式
UseBridges 1

# 指定 obfs4 插件路径
ClientTransportPlugin obfs4 exec D:\Tor_Expert\tor\pluggable_transports\obfs4proxy.exe

# 添加 obfs4 桥接（从官方或社区获取）
Bridge obfs4 157.180.44.154:30366 8BB8B3615A72A1289481C5CFC9F9B9DE8668A87A cert=+iqyeW8f0r73HK0xiDjc1CjX5IGW2jQP5Mi7AWi0oTweraKBgua8hRtaHqD/lTNnfUgofA iat-mode=0
Bridge obfs4 157.180.41.252:61453 340005A22F1559D25B4F74DE4518901CB34CEB72 cert=29Fld7O2XPD+jp8EjWGwC9gkSaqpL7OkdjwAgQ1Q/O5dR8zTuAD2EZUCq5HmexmTx7CqDw iat-mode=0

# 出口节点限制
ExcludeNodes {cn},{ir},{ru}
StrictNodes 1

# 性能优化
AvoidDiskWrites 1
CircuitBuildTimeout 10
NumEntryGuards 5
LongLivedPorts 80,443

# GeoIP 文件路径
GeoIPFile D:\Tor_Expert\data\geoip
GeoIPv6File D:\Tor_Expert\data\geoip6




新版的tor好像不再提供obf4proxy.exe,Tor browser和Tor expert Bunder 都没有,需要去GitHub上拉取它的go文件,这里我在unbuntu虚拟机下搭建go和gcc环境对文件进行编译为.exe最后放在pluggable_transports目录下 随后tor.exe -f torcc 成功开启tor服务,这里可以写个脚本定时去telegram上tor的机器人获取网桥,多备几个入点防止被被封,之后又去手动获取