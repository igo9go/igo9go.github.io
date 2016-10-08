---
title: 为你总结一些php系统类函数
date: 2016-09-27 14:28:21
tags: PHP
categories: PHP
---
#PHP的一些系统函数

1. assert函数：检查assertion声明是否错误
2. extension_loaded函数：检查PHP扩展是否加载
3. get_cfg_var函数：获取PHP配置选项的值
4. get_current_user函数：获取当前PHP脚本的所有者的名称
5. get_defined_constants函数：返回一个包含PHP预定义常量信息的数组
6. get_extension_funcs函数：返回一个包含指定模块中的所有函数名称的数组
7. get_include_path函数：返回当前配置的文件包含路径的信息
8. get_included_files函数：返回一个关于文件包含信息的数组
9. get_loaded_extensions函数：返回一个包含所有装载模块信息的数组
1. get_magic_quotes_gpc函数：获取magic_quotes_gpc的状态信息
2. get_magic_quotes_runtime函数：获取magic_quotes_ runtime的状态信息
3. get_required_files函数：返回一个关于文件包含信息的数组
4. getenv函数：获取PHP环境变量的值

5. getlastmod函数：获取当前PHP页面文件的最后修改时间
6. getmygid函数：获取当前PHP脚本页面所有者的GID号码
7. getmyinode函数：获取当前PHP脚本页面的INODE号码
8. getmypid函数：获取PHP的PID
9. getmyuid函数：获取PHP脚本页面所有者的UID号码
1. getopt函数：从命令行参数列表获取设置
2. getrusage函数：获取当前的资源语法
3. ini_get_all函数：获取所有配置选项
4. ini_get函数：获取配置选项的值
5. memory_get_usage函数：返回PHP脚本占用的内存空间
6. php_ini_scanned_files函数：返回配置文件目录下的配置文件列表
7. php_logo_guid函数：获取LOGO图片的GUID
8. php_sapi_name函数：获取PHP和Web服务器之间的接口类型
9. php_uname函数：获取PHP脚本运行的操作系统信息
1. phpcredits函数：打印credits列表
2. phpinfo函数：输出PHP的信息
3. phpversion函数：获取PHP版本
4. zend_logo_guid函数：获取ZEND的LOGO图片的GUID
5. zend_version函数：获取ZEND引擎的版本

6. assert_options函数：设置或者获取不同的声明标记
7. ini_alter函数：设置PHP配置选项的值
8. ini_restore函数：恢复配置选项的值
9. ini_set函数：设置PHP配置选项的值
1. putenv函数：设置环境变量
2. restore_include_path函数：恢复文件包含路径配置信息
3. set_include_path函数：设置文件包含路径配置选项
4. set_magic_quotes_runtime函数：设置magic_quotes运行时间
5. set_time_limit函数：设置最大执行时间

