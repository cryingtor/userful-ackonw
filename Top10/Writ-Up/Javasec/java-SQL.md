# java-SQL
预编译:  
JDBC:?占位符
Mybatis:#预编译,$拼接
Hibernate
弱点:
JDBC:未使用prepreperd或未使用?占位符,导致预编译失效
Mybatis:使用${}拼接
局限性
* ​动态表名/列名场景
    当需要动态指定表名或列名（如 ORDER BY ${column}）时，必须使用 ${}，此时需手动验证参数合法性（如白名单过滤）。
* ​模糊查询中的误用
    若将模糊查询写成 LIKE '%${keyword}%'，参数会直接拼接，导致注入风险。正确做法是使用 LIKE CONCAT('%', #{keyword}, '%')。
* ​IN 语句的动态参数
    使用 IN (${ids}) 直接拼接参数时，需确保参数合法性（如拆分字符串为列表，并用 #{} 遍历）。
* ​框架配置或编码错误
    若开发者错误地混合使用字符串拼接（如 +）与占位符，或未正确配置 MyBatis，仍可能引入漏洞

Hibernate:
* 动态 HQL 拼接
若开发者手动拼接 HQL（如 String hql = "FROM User WHERE name = '" + userInput + "'"），会导致注入漏洞
* 原生 SQL 的不当使用
直接执行未参数化的原生 SQL（如 session.createSQLQuery("SELECT * FROM users WHERE id = " + id)）会绕过 Hibernate 的防护机制
* 动态排序（ORDER BY）或表名
当动态指定排序字段或表名时，需使用 ${} 拼接
























