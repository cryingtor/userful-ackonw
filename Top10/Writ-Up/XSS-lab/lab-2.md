# lab-2
发现输入结果在前端返回显示,应该是反射性XSS,
```
<script>alert('1')</script>
```
没有弹窗
查看源码,
![](vx_images/322470583619978.png)
观察发现,<>被过滤,
查阅源码发现被htmlspecialchars($str)过滤
使用"><script>alert('xss')</script>//
![](vx_images/191583836105129.png)
