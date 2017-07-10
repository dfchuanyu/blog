# 标量类型声明
标量类型声明 有两种模式: 强制 (默认) 和 严格模式。 现在可以使用下列类型参数（无论用强制模式还是严格模式）： 字符串(string), 整数 (int), 浮点数 (float), 以及布尔值 (bool)。
它们扩充了PHP5中引入的其他类型：类名，接口，数组和 回调类型。
```
<?php
// Coercive mode
function sumOfInts(int ...$ints)
{
    return array_sum($ints);
}

var_dump(sumOfInts(2, '3', 4.1));   //int(9)

```
要使用严格模式，一个 declare 声明指令必须放在文件的顶部。这意味着严格声明标量是基于文件可配的。 这个指令不仅影响参数的类型声明，也影响到函数的返回值声明
# 返回值类型声明
PHP 7 增加了对返回类型声明的支持。 类似于参数类型声明，返回类型声明指明了函数返回值的类型。可用的类型与参数声明中可用的类型相同。
```
<?php

function arraysSum(array ...$arrays): array
{
    return array_map(function(array $array): int {
        return array_sum($array);
    }, $arrays);
}

print_r(arraysSum([1,2,3], [4,5,6], [7,8,9]));

以上例程会输出：

Array
(
    [0] => 6
    [1] => 15
    [2] => 24
)
```
# null合并运算符
由于日常使用中存在大量同时使用三元表达式和 isset()的情况， 我们添加了null合并运算符 (??) 这个语法糖。
如果变量存在且值不为NULL， 它就会返回自身的值，否则返回它的第二个操作数。
```
<?php
// Fetches the value of $_GET['user'] and returns 'nobody'
// if it does not exist.
$username = $_GET['user'] ?? 'nobody';
// This is equivalent to:
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';

// Coalesces can be chained: this will return the first
// defined value out of $_GET['user'], $_POST['user'], and
// 'nobody'.
$username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';
```
# 太空船操作符（组合比较符)
```
<?php
// 整数
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// 浮点数
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
```
# 通过 define() 定义常量数组
```
<?php
define('ANIMALS', [
    'dog',
    'cat',
    'bird'
]);

echo ANIMALS[1]; // 输出 "cat"
```
# 匿名类 
现在支持通过new class 来实例化一个匿名类，这可以用来替代一些“用后即焚”的完整类定义。
```
<?php
interface Logger {
    public function log(string $msg);
}

class Application {
    private $logger;

    public function getLogger(): Logger {
         return $this->logger;
    }

    public function setLogger(Logger $logger) {
         $this->logger = $logger;
    }
}

$app = new Application;
$app->setLogger(new class implements Logger {
    public function log(string $msg) {
        echo $msg;
    }
});

var_dump($app->getLogger());

以上例程会输出：

object(class@anonymous)#2 (0) {
}
```
# Closure::call()
Closure::call() 现在有着更好的性能，简短干练的暂时绑定一个方法到对象上闭包并调用它。
```
<?php
class A {private $x = 1;}

// PHP 7 之前版本的代码
$getXCB = function() {return $this->x;};
$getX = $getXCB->bindTo(new A, 'A'); // 中间层闭包
echo $getX();   //1

// PHP 7+ 及更高版本的代码
$getX = function() {return $this->x;};
echo $getX->call(new A);    //1
```
# Group use declarations
从同一 namespace 导入的类、函数和常量现在可以通过单个 use 语句 一次性导入了。
```
<?php

// PHP 7 之前的代码
use some\namespace\ClassA;
use some\namespace\ClassB;
use some\namespace\ClassC as C;

use function some\namespace\fn_a;
use function some\namespace\fn_b;
use function some\namespace\fn_c;

use const some\namespace\ConstA;
use const some\namespace\ConstB;
use const some\namespace\ConstC;

// PHP 7+ 及更高版本的代码
use some\namespace\{ClassA, ClassB, ClassC as C};
use function some\namespace\{fn_a, fn_b, fn_c};
use const some\namespace\{ConstA, ConstB, ConstC};
```
# 整数除法函数 intdiv()
新加的函数 intdiv() 用来进行 整数的除法运算。
```
<?php
var_dump(intdiv(10, 3));    //int(3)
```

[更多新特性](http://php.net/manual/zh/migration70.new-features.php)
