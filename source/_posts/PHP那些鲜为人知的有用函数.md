---
title: PHP那些鲜为人知的有用函数
date: 2016-09-23 17:47:16
tags: PHP
categories: PHP
---

1. levenshtein()

    你有没有经历过需要知道两个单词有多大的不同的时候，这个函数就是来帮你解决这个问题的。它能比较出两个字符串的不同程度。

    ```
    <?php
    $str1 = "carrot";
    $str2 = "carrrott";
    echo levenshtein($str1, $str2); //Outputs 2
    ?>
    ```
    > Source: http://php.net/manual/en/function.levenshtein.php
2. get_defined_vars()
    这是一个在debug调试时非常有用的函数。这个函数返回一个多维数组，里面包含了所有定义过的变量。
   
    ```
   <?php
   print_r(get_defined_vars());
   ?>
   ```
    >Source: http://php.net/manual/en/function.get-defined-vars.php
3.  ignore_user_abort()

    这个函数用来拒绝浏览器端用户终止执行脚本的请求。正常情况下客户端的退出会导致服务器端脚本停止运行。
    ```
    <?php
    ignore_user_abort();
    ?>
    ```
    > Source: http://www.php.net/manual/en/function.ignore-user-abort.php
4. highlight_string()

    你想把PHP代码显示到页面上时，highlight_string()函数就会显得非常有用。这个函数会把你提供的PHP代码用内置的PHP语法突出显示定义的颜色高亮显示。这个函数有两个参数，第一个参数是一个字符串，表示这个字符串需要被突出显示。第二个参数如果设置成TRUE，这个函数就会把高亮后的代码当成返回值返回。

    ```
    <?php
        highlight_string(' <?php phpinfo(); ?>');
    ?>
    ```
    
    > Source: http://php.net/manual/en/function.highlight-string.php
5. highlight_file

这是一个非常有用的PHP函数，它能返回指定的PHP文件，并按照语法语义用高亮颜色突出显示文件内容。其中的突出显示的代码都是用HTML标记处理过的。

用法：

<?php

highlight_file("php_script.php");

?>

Source: http://www.php.net/manual/en/function.highlight-file.php

6. php_strip_whitespace

这个函数也跟前面的show_source()函数相似，但它会删除文件里的注释和空格符。

用法：

<?php

echo php_strip_whitespace("php_script.php");

?>

Source: http://www.php.net/manual/en/function.php-strip-whitespace.php



http://www.chinaz.com/program/2014/0128/337438.shtml



