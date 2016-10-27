---
title: golang学习笔记
date: 2016-10-25 10:07:53
tags:
categories:
---

- 变量必须预先申明定义，变量会在声明时直接初始化。如果变量没有显式初始化，则被隐式地赋予其类型的零值（zero value），数值类型是0，字符串类型是空字符串""。申明了变量就必须引用
- Go语言只有for循环这一种循环语句

    1. 普通的循环
    
        ```
        for initialization; condition; post {
        // zero or more statements
        }
        ```
    
    2. 类似while的循环
    
        ```
        // a traditional "while" loop
        for condition {
        // ...
        }
        ```
    
    3. 死循环
    
        ```
        // a traditional infinite loop
        for {
            // ...
        }
        ```
    4. for循环的另一种形式, 在某种数据类型的区间（range）上遍历，如字符串或切片
    
        ```
        for _, arg := range os.Args[1:] {
            s += sep + arg
            sep = " "
        }
        ```
        每次循环迭代，range产生一对值；索引以及在该索引处的元素值。这个例子不需要索引，但range的语法要求, 要处理元素, 必须处理索引。

- 变量申明
    
    ```
    s := ""
    var s string
    var s = ""
    var s string = ""
    ```

- 25个关键字

    break      default       func     interface   select
    case       defer         go       map         struct
    chan       else          goto     package     switch
    const      fallthrough   if       range       type
    continue   for           import   return      var

- 预定义名称

    内建常量: true false iota nil

    内建类型: int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error

    内建函数: make len cap new append copy close delete
          complex real imag
          panic recover
- Go语言主要有四种类型的声明语句：var、const、type和func，分别对应变量、常量、类型和函数实体对象的声明

- 变量申明
    
    var 变量名字 类型 = 表达式
 

如果用“var x int”声明语句声明一个x变量，那么&x表达式（取x变量的内存地址）将产生一个指向该整数变量的指针，指针对应的数据类型是*int，指针被称之为“指向int类型的指针”。如果指针名字为p，那么可以说“p指针指向变量x”，或者说“p指针保存了x变量的内存地址”。同时*p表达式对应p指针指向的变量的值。一般*p表达式读取指针指向的变量的值，这里为int类型的值，同时因为*p对应一个变量，所以该表达式也可以出现在赋值语句的左边，表示更新指针所指向的变量的值。

- 常量声明可以使用iota常量生成器初始化，它用于生成一组以相似规则初始化的常量，但是不用每行都写一遍初始化表达式。在一个const声明语句中，在第一个声明的常量所在的行，iota将会被置为0，然后在每一个有常量声明的行加一

- r := [...]int{99: -1}
    定义了一个含有100个元素的数组r，最后一个元素被初始化为-1，其它元素都是用0初始化。+




