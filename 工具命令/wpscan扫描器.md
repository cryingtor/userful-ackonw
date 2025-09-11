# wpscan扫描器
扫描网站
```
wpscan --url https://example.com
```

---

```
枚举扫描（关键参数）：
参数	功能	示例
--enumerate u	枚举用户名	wpscan --url https://ex.com --enumerate u
--enumerate p	枚举插件	wpscan --url https://ex.com --enumerate p
--enumerate t	枚举主题	wpscan --url https://ex.com --enumerate t
--enumerate vp	扫描有漏洞的插件	wpscan --url https://ex.com --enumerate vp
--enumerate vt	扫描有漏洞的主题	wpscan --url https://ex.com --enumerate vt
--api-token <TOKEN>	使用 WPScan API 获取实时漏洞库	需注册 wpscan.com 获取 Token
```


----

```
完整扫描（用户+漏洞插件+爆破）：
wpscan --url https://vuln-wp-site.com \
  --enumerate u,vt,vp \
  --passwords /usr/share/wordlists/rockyou.txt \
  --api-token YOUR_API_TOKEN \
  --random-user-agent \
  -o report.txt

```