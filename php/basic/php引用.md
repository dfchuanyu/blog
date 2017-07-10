# php foreach使用引用的陷阱
在foreach中使用引用的时候出现一个怪现象,使用2次foreach的时候数组值发生了改变，代码示例如下
```
<?php
$arr = array('1','2','3');
foreach($arr as &$row){
}
foreach($arr as $row){
}
```
预期结果是1,2,3 但是实际结果输出1,2,2<br>
在第2个循环里打印$arr
```
Array
(
[0] => 1
[1] => 2
[2] => 1
)
Array
(
[0] => 1
[1] => 2
[2] => 2
)
Array
(
[0] => 1
[1] => 2
[2] => 2
)
```
1. 第1个foreach结束，$row成为$arr[2]的引用
2. 第2个foreach循环第1次的时候$row=1,所以此时$arr[2]=1,所以此时$arr=array(1,2,1),
3. 依次类推,第2个foreach循环结束的时候$row=2,所以此时$arr=array(1,2,2);

# 其它引用
```
<?php
$array1 = array(1,2);
$x = &$array1[1]; // Unused reference
$array2 = $array1; // reference now also applies to $array2 !
$array2[1]=22; // (changing [0] will not affect $array1)
print_r($array1);

Produces:
Array(
  [0] => 1
  [1] => 22 // var_dump() will show the & here
)
```
结果又出乎意料，注释掉$x = &$array1[1],改变$array2的值不影响$arry1的值,联想到php垃圾回收的引用计数器，每个php变量存在于zval容器里，
zval容器除了包含变量的值和类型，还有2个额外的信息，第1个是is_ref,是个bool值，用来标识变量是否是引用集合，通过这个我就知道这个变量是否
是普通变量或者引用变量啦，第2个额外字段是refcount,表示指向这个zval容器的变量个数。<br>
zval结构如下
```
typedef struct _zval_struct zval;
...
struct _zval_struct {
/* Variable information */
zvalue_value value; /* value */
zend_uint refcount__gc;
zend_uchar type; /* active type */
zend_uchar is_ref__gc;
};
```
[引用地址](http://www.cnblogs.com/sklyn/p/5559008.html)
