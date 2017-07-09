# 大小写敏感
1. 变量大小写敏感。包括普通变量以及$_GET,$_POST,$_REQUEST,$_COOKIE,$_SESSION,$GLOBALS,$_SERVER,$_FILES,$_ENV 等；
```
<?php
$abc = 'abcd';
echo $abc; //输出 'abcd'
echo $aBc; //无输出
echo $ABC; //无输出
```
2. 常量名默认区分大小写，通常都写为大写
```
<?php
define("ABC","Hello World");
echo ABC;   //输出 Hello World
echo abc;   //输出 abc
```
3. php.ini配置项指令区分大小写,如 file_uploads = 1 不能写成 File_uploads = 1
# 大小写不敏感
1. 函数名、方法名、类名 不区分大小写，但推荐使用与定义时相同的名字
```
<?php
function show(){
  echo "Hello World";
}
show(); //输出 Hello World    推荐写法
SHOW(); //输出 Hello World
 
class cls{
  static function func(){
    echo "hello world";
  }
}	 
Cls::FunC();  //输出hello world
```
2. 魔术常量不区分大小写，推荐大写,包括：__LINE__、__FILE__、__DIR__、__FUNCTION__、__CLASS__、__METHOD__、 __NAMESPACE__。
```
<?php
echo __line__;  //输出 2
echo __LINE__;  //输出 3
```
3. NULL、TRUE、FALSE不区分大小写
```
<?php
$a = null;
$b = NULL; 
$c = true;
$d = TRUE;
$e = false;
$f = FALSE;
	 
var_dump($a == $b); //输出 boolean true
var_dump($c == $d); //输出 boolean true
var_dump($e == $f); //输出 boolean true
```
4.类型强制转换，不区分大小写,包括
(int)，(integer) – 转换成整型
(bool)，(boolean) – 转换成布尔型
(float)，(double)，(real) – 转换成浮点型
(string) – 转换成字符串
(array) – 转换成数组
(object) – 转换成对象
