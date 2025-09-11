# dirsearch
-u 地址
--cookie 指定cookie(常用)
--w 指定字典

```
参数增强手册（实战技巧）
参数	缩写	使用频率	作用说明	高危场景示例
-t	-	⭐⭐	线程数（默认25）	-t 50 加速扫描
--proxy	-	⭐⭐⭐	SOCKS/HTTP代理	--proxy socks5://127.0.0.1:1080
-H	-	⭐⭐⭐⭐	自定义请求头	-H 'X-Forwarded-For: 127.0.0.1'
-r	-	⭐	递归扫描	配合 --scan-subdirs 使用
-F	-	⭐⭐	强制扩展名	-F .php 强制所有路径加后缀
​**--skip-on-status**​	-	⭐⭐⭐	跳过指定状态码	--skip-on-status 302,429
​**--minimal**​	-	⭐⭐⭐⭐	只显示有效结果	避免日志淹没
```

组合

.​后台爆破 + Cookie 保持
```
dirsearch -u https://admin.corp.com \ 
--cookie "JSESSIONID=ADF9078A5F91" \ 
-e asp,aspx,jsp \ 
--scan-subdirs
```








