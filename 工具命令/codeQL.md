# codeQL
# CodeQL 分析代码主要分为两个核心步骤：创建数据库和分析数据库
```
# 1. 创建数据库：将源代码转换为 CodeQL 可查询的数据库
codeql database create <database_path> --language=<language> --source-root=<source_code_path> [--command=<build_command>]

# 2. 分析数据库：运行查询以发现漏洞
codeql database analyze <database_path> <queries_or_packs> --format=<format> --output=<result_file>
```

--- 

# java 项目检测命令
创建数据库
对于需要编译的 Java 项目，通常需要指定构建命令
```
# 对于 Maven 项目
codeql database create java-db --language=java --source-root=/path/to/java/project --command="mvn clean compile -DskipTests"

# 对于 Gradle 项目
codeql database create java-db --language=java --source-root=/path/to/java/project --command="gradle build -x test"

# 如果项目已编译或无需编译，可省略 --command
codeql database create java-db --language=java --source-root=/path/to/java/project
```
运行安全查询
```
# 使用官方安全扩展查询包（推荐，包含多种漏洞检测）
codeql database analyze java-db codeql-suites/java-security-extended.qls --format=csv --output=java-security-results.csv

# 或指定具体漏洞类型的查询文件
codeql database analyze java-db java/ql/src/Security/CWE/CWE-089/SqlInjection.ql --format=sarif-latest --output=java-sql-injection.sarif
```

--- 

# Python 项目检测命令

创建数据库
Python 是脚本语言，通常无需编译命令
```
# 为 Python 项目创建数据库
codeql database create python-db --language=python --source-root=/path/to/python/code
```
运行安全查询
```
# 运行 Python 安全扩展查询
codeql database analyze python-db codeql-suites/python-security-extended.qls --format=csv --output=python-security-results.csv

# 检测命令注入
codeql database analyze python-db python/ql/src/Security/CWE/CWE-078/CommandInjection.ql --format=csv --output=python-command-injection.csv
```

---

# PHP 项目检测命令
创建数据库
```
# 为 PHP 项目创建数据库
codeql database create php-db --language=php --source-root=/path/to/php/code
```
运行安全查询
```
# 运行 PHP 安全扩展查询
codeql database analyze php-db codeql-suites/php-security-extended.qls --format=csv --output=php-security-results.csv

# 检测 SQL 注入
codeql database analyze php-db php/ql/src/Security/CWE/CWE-089/SqlInjection.ql --format=csv --output=php-sql-injection.csv
```

---

解决常见错误
```
问题：is not a .ql file, .qls file, a directory, or a query pack specification

解决方案：使用查询套件（.qls）或查询包名称，而非直接指定可能不存在的具体 .ql 文件路径。
```