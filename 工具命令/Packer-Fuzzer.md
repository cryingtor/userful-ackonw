# Packer-Fuzzer
```
1. 忽略特定文件类型
python3 PackerFuzzer.py -u http://example.com --exclude .jpg,.png


2.强制使用代理调试
python3 PackerFuzzer.py -u http://example.com --proxy http://127.0.0.1:8080

3.绕过 HTTPS 证书验证
python3 PackerFuzzer.py -u https://example.com --no-check-certificate
```