---
title: PHP各版本特性
date: 2016-08-23 22:10:21
tags:
categories:
---
#PHP各版本特性
###  PHP5.6 
1. 可以使用表达式定义常量

    https://php.net/manual/zh/migration56.new-features.php
    在之前的 PHP 版本中，必须使用静态值来定义常量，声明属性以及指定函数参数默认值。 现在你可以使用包括数值、字符串字面量以及其他常量在内的数值表达式来 定义常量、声明属性以及设置函数参数默认值。
    <pre>
    <?php
    const ONE = 1;
    const TWO = ONE * 2;    //定义常量时允许使用之前定义的常量进行计算
    class C {
        const THREE = TWO + 1;
        const ONE_THIRD = ONE / self::THREE;
        const SENTENCE = 'The value of THREE is '.self::THREE;
        public function f($a = ONE + self::THREE) { //允许常量作为函数参数默认值
            return $a;
        }
    }
    echo (new C)->f()."
    ";
    echo C::SENTENCE;
    ?> 
    </pre>
    
    
    可以通过 const 关键字来定义类型为 array 的常量。
    <pre>
    <?php
    const ARR = ['a', 'b'];
    echo ARR[0];
    ?>
    </pre>

2. 使用 ... 运算符定义变长参数函数
    
    现在可以不依赖 func_get_args()， 使用 ... 运算符 来实现 变长参数函数。
    <pre>
    <?php
    function test(...$args)
    {
        print_r($args);
    }
    
    test(1,2,3);
    //输出
    Array
    (
        [0] => 1
        [1] => 2
        [2] => 3
    )
    ?>
    </pre>
    
3. 使用 ** 进行幂运算

    加入右连接运算符 ** 来进行幂运算。 同时还支持简写的 **= 运算符，表示进行幂运算并赋值。
    
    <pre>
    printf(2 ** 3); // 8
    $a = 2;
    $a **= 3;
    printf($a);  // 8
    </pre>

4. use function 以及 use const

    use 运算符可以在类中导入外部的函数和常量了。 对应的结构为 use function 和 use const。
    
  
    <?php
    namespace NameSpace {
        const FOO = 42;
        function f() { echo __FUNCTION__.""; }
    }
    namespace {
        use const NameSpaceFOO;
        use function NameSpacef;
        echo FOO."";
        f();    
    }
    ?>

5. 加入 hash_equals() 函数，以恒定的时间消耗来进行字符串比较，以避免时序攻击
   
    <?php
    $expected  = crypt('12345', '$2a$07$usesomesillystringforsalt$');
    $incorrect = crypt('1234',  '$2a$07$usesomesillystringforsalt$');
    
    var_dump(hash_equals($expected, $incorrect)); // false
    ?>

6. 加入 __debugInfo()
    当使用 var_dump() 输出对象的时候，可以用来控制要输出的属性和值。
   
   <pre>
    <?php
    class C {
        private $prop;
        public function __construct($val) {
            $this->prop = $val;
        }
        public function __debugInfo() {
            return $this->prop;
        }
    }
    var_dump(new C(42));
    ?>
    </pre>

### PHP 5.5
1. 新增 Generators

    yield关键字用于当函数需要返回一个迭代器的时候，逐个返回值。
    function number10()
    {
        for($i = 1; $i <= 10; $i += 1)
            yield $i;
    } 该函数的返回值是一个数组：
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
2. 新增 finally 关键字 Finally处理流程

3. foreach 支持 list() foreach 支持通过 list() 将嵌套数组分离到单独的变量。

   
    <?php
    $array = [
        [1, 2],
        [3, 4],
    ];
    
    foreach ($array as list($a, $b)) {
        echo $a.$b;
    }
    ?>
    
4. empty() 支持传入一个任意表达式，而不仅是一个变量

    <?php
    function always_false() {
        return false;
    }
    
    if (empty(always_false())) {
        echo 'This will be printed.';
    }
    
5. 直接通过下标获取访问数组和字符串字面量的元素或字符
    
    echo [1, 2, 3][0]; // 1
    echo 'PHP'[0]; // P
6. 新的密码哈希 API

    缺点是缺乏互操作性，在需要和其他语言对接时会比较麻烦。
    //加密
    echo $hash = password_hash('rasmuslerdorf', PASSWORD_DEFAULT);
    //输出结果类似于：$2y$10$.vGA1O9wmRjrwAVXD98HNOgsNpDczlqm3Jq7KnEd1rVAGv3Fykk1a
    
    //验证
    if(password_verify('rasmuslerdorf','$2y$10$.vGA1O9wmRjrwAVXD98HNOgsNpDczlqm3Jq7KnEd1rVAGv3Fykk1a')) { 
        echo "密码正确";
    } else {  
        echo "密码错误";
    }

7. 新增 boolval() 函数
    
    PHP已经实现了strval、intval和floatval的函数。为了达到一致性将添加boolval函数。
8. 新增 array_column() 函数

    可用来返回数组中指定的一列。
    $records = array(
        array('id' => 2135,'name' => 'John'),
        array('id' => 3245,'name' => 'Smith'),
        array('id' => 5342,'name' => 'Peter')
    );
    
    //从结果集中取出 name 列
    $names = array_column($records, 'name');
    print_r($names);
    
    //从结果集中总取出 name 列，用相应的 id 作为键值
    $names = array_column($records, 'name', 'id');
    print_r($names);
### PHP 5.4
1. 新增 Traits
    // Traits不能被单独实例化，只能被类所包含
    trait SayWorld
    {
        public function sayHello()
        {
            echo 'World!';
        }
    }
    
    class MyHelloWorld
    {
        // 将SayWorld中的成员包含进来
        use SayWorld;
    }
    
    $xxoo = new MyHelloWorld();
    // sayHello() 函数是来自 SayWorld 构件的
    $xxoo->sayHello();
2. 新增短数组语法

    // 原来的数组写法
    $arr = array("key" => "value", "key2" => "value2");
    $arr = array(1,2,3,4);
    // 简写形式
    $arr = ["key" => "value", "key2" => "value2"];
    $arr = [1,2,3,4];

3. 新增支持对函数返回数组的成员访问解析

    print func()[0];

4. 无论 php.ini 中是否设置 short_open_tag， 格式总是可用。

    这种简写形式被称为 Short Open Tag, 在 PHP5.3 起被默认开启，在 PHP5.4 起总是可用。 使用这种简写形式在 HTML 中嵌入 PHP 变量将会非常方便。
    
5. 内置用于开发的 CLI 模式的 web server
    //启动Web服务器
    php -S localhost:8000
    //启动时指定根目录
    php -S localhost:8000 -t /home/me/public_html/foo
    //使用路由（Router）脚本
    php -S localhost:8000 index.php //所有的请求都会由index.php来处理。6、新增在实例化时访问类成员
     (new Foo)->bar();7、新增了动态访问静态方法的方式
    $func = "funcXXOO";
    A::{$func}();
8. 闭包支持 $this 

9. 新增二进制直接量

    $bin = bindec(110011); //之前需要这样写
    $bin = 0b110011;
    echo $bin; //51

10. session提供了上传进度支持

    通过 $_SESSION["upload_progress_name"] 就可以获得当前文件上传的进度信息，结合 Ajax 就能很容易的实现上传进度条。
    
11. 默认使用 mysqlnd

    现在mysql, mysqli, pdo_mysql默认使用mysqlnd本地库，在PHP5.4以前需要：./configure --with-mysqli=mysqlnd
    现在：./configure --with-mysqli
12. 让 json 更懂中文

    echo json_encode("中文", JSON_UNESCAPED_UNICODE);  
    //"中文" 13、default_charset从ISO-8859-1已经变为UTF-8
    默认发送“Content-Type: text/html; charset=utf-8”
### PHP 5.3
1、支持命名空间
<?php
namespace my
ame; // 定义命名空间

class MyClass {}
const MYCONST = 1;

$a = new my
ameMyClass;
$class = __NAMESPACE__.'MyClass';
$b = new $class;

$c = my
ameMYCONST;
$d = namespaceMYCONST;

var_dump($a,$b,$c,$d);
/*
object(my
ameMyClass)#1 (0) {
}
object(my
ameMyClass)#2 (0) {
}
int(1)
int(1)
*/
?>2、增加后期静态绑定
在PHP中，我们可以在类中通过self关键字或者CLASS来判断或调用当前类。但有一个问题，如果我们是在子类中调用，得到的结果将是父类。因为在继承父类的时候，静态成员就已经被绑定了。
class A
{
    static public function callFuncXXOO()
    {
        print self::funcXXOO();
    }

    static public function funcXXOO()
    {
        return "A::funcXXOO()";
    }
}

class B extends A
{
    static public function funcXXOO()
    {
        return "B::funcXXOO";
    }
}

$b = new B;
$b->callFuncXXOO();
//输出
A::funcXXOO() PHP 5.3.0中增加了一个static关键字来引用当前类，即实现了延迟静态绑定：
class A
{
    static public function callFuncXXOO()
    {
        print static::funcXXOO();
    }

    // ...
}
// ...
//这样就会像预期一样输出了：
B::funcXXOO
3、增加 goto 操作符
goto 语句有可能会导致程序流程不清晰，可读性减弱，但在某些情况下具有其独特的方便之处，例如中断深度嵌套的循环和 if 语句。
<?php
goto test;
echo '1';
 
test:
echo '2';
?>

//以上运行时会输出 2
4、添加了原生的闭包(Lambda/匿名函数)支持
https://php.net/manual/zh/functions.anonymous.php
5、新增两个魔术方法, __callStatic 和 __invoke
用静态方式中调用一个不可访问方法时，__callStatic() 会被调用。
当尝试以调用函数的方式调用一个对象时，__invoke() 方法会被自动调用。
class A
{
    public function __invoke($str)
    {
        print "A::__invoke(): {$str}";
    }
}

$a = new A;
$a("Hello World");
//输出
A::__invoke(): Hello World6、添加 Nowdoc 语法支持
<?php
$str = <<<'EOD'
Example of string
spanning multiple lines
using nowdoc syntax.
EOD;
就象 heredoc 结构类似于双引号字符串，Nowdoc 结构是类似于单引号字符串的。Nowdoc 结构很象 heredoc 结构，但是 nowdoc 中不进行解析操作。
7、Heredoc 结构中可以用双引号来声明标识符了。
<?php
echo <<<"FOOBAR"
Hello World!
FOOBAR;
?>8、const 关键字可用来在类定义之外定义常量了
<?php
define("CONSTANT_A", "Hello world");
const CONSTANT_B = 'Hello World'; const 形式仅适用于常量，不适用于运行时才能求值的表达式：
// 正确
const XXOO = 1234;
// 错误
const XXOO = 2 * 617; 和使用 define() 来定义常量不同的是，使用 const 关键字定义常量必须处于最顶端的作用域，因为用此方法是在编译时定义的。即不能在函数内，循环内以及 if 语句之内用 const 来定义常量。

9、三元运算符可以简写省略中间的部分
表达式 expr1 ?: expr3 ，当 expr1 为 TRUE 时返回 expr1，否则返回 expr3。
10、异常可以嵌套了
<?php

class MyException extends Exception { }

class Test {
    public function testing() {
        try {
            try {
                throw new MyException('foo!');
            } catch (MyException $e) {
                /* rethrow it */
                throw $e;
            }
        } catch (Exception $e) {
            var_dump($e->getMessage());
        }
    }
}

$foo = new Test;
$foo->testing();
?>11、可以动态访问静态变量了
<?php
class C {
   public static $foo = 123;
}

$a = "C";
echo $a::$foo;
?> 上边运行时输出：123
12、mail()函数支持记录发送日志了
在配置文件 php.ini 中可设置日志路径。参数名：mail.log
参考资料
https://php.net/manual/zh/migration53.new-features.php
https://php.net/manual/zh/migration54.new-features.php
https://php.net/manual/zh/migration55.new-features.php
https://php.net/manual/zh/migration56.new-features.php
http://segmentfault.com/a/1190000000403307
http://blog.csdn.net/heiyeshuwu/article/details/16884725
http://segmentfault.com/a/1190000002790818

