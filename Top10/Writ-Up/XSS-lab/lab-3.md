# lab-3
输入<script>alert('1')</script>,没有弹窗
尝试"><script>alert('xss')</script>//,结果不行,但是发现输入框的内容缺失了
![](vx_images/55024092849542.png)
测试1',发现'被过滤
查看源码,发现对输入的值进行了两次转义,用事件触发
'onmouseover='alert(1)
'onfocus=javascript:alert(1)>//