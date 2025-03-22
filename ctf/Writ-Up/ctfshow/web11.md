# web11
```
<?php
        function replaceSpecialChar($strParam){
             $regex = "/(select|from|where|join|sleep|and|\s|union|,)/i";
             return preg_replace($regex,"",$strParam);
        }
        if(strlen($password)!=strlen(replaceSpecialChar($password))){
            die("sql inject error");
        }
        if($password==$_SESSION['password']){
            echo $flag;
        }else{
            echo "error";
        }
    ?>
```

题目逻辑:从传入的session中的password与传入的password比较,password进行了正则过滤,过滤前后需保持一致



