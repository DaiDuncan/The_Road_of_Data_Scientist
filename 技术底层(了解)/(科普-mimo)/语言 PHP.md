定位：服务端server-side的语言(和JavaScript相差很大)

- 所有的PHP脚本都需要运行在server上
- 只有脚本的==结果==才会显示在网页上



作用：

- 与数据库通信
- 创建动态网页
- 从表单中提取用户的输入



应用企业：

- Facebook
- Tesla

```php
<?php	//opening tag
    echo "Welcome, friend!";
?>		//closing tag
```





## 与HTML相结合

生成动态页面

```php+HTML
<html>
    <body>
        <?php
	        echo "Hi!";
        ?>
    </body>
</html>
```

文件名后缀是.php：向服务器表明，需要处理该文件



# 变量及其类型

```php
<?php
    $name1 = "Slim Shady";
	$name2 = "Mike";
	echo $name1;
	echo $name1 . $name2;	//.作用等同于字符串的+

	define("PI", 3.14);
	echo PI;	//对于常数来说，不需要$
?>
```

## 布尔值

```php
$boolean = true;	//打印的是数字1，false则是0
```

逻辑运算符：

- &&
- ||
- !<var>





# 语句

## 条件

```php
if(true) {
    echo "Hi";
}

// 结构
if(){
    
}elseif(){
    
}else{
    
}

//结构switch
$season = "Winter";
switch($season){
    case "Winter":
        break;
        ...;
    default:
           
}
```





## 循环

```php
while(){
    
}


do{
    
}while();



for($i = 0; $i < 4; $i++){
    
}


//
$heroes = array("A", "B", "C");
foreach($heroes as $hero){
    echo $hero . ", ";
}
```







# 数据结构

## 数组Array constructor

- 一种类型的数据

```php
$friends = array("A", "B");
$friends[] = "C";	//添加新的值：默认添加到后面
$friends[2] = "C";	//即便index不存在，也能添加，但是不能访问 => 比如$friends[3]会报错

var_dump($friends);	//输出整个数组

//数组方法
count($<array>);
sort($<>);

    
```







# 函数

```php
function sayHi(){
    echo "Hi";
}


function sum($n1, $n2){
    $sum = $n1 + $n2;
    return $sum;	//不能直接return $n1 + $n2, 这是一个新的变量
}
echo "1 + 7 = " .sum(1, 7);
```



## 内置函数

```php
strtolower(<str>);
str_replace(<str_old>, <str_new>, <origin_str>);
substr(<str>, <begin_index>, <num>);
str_word_count();
round();
explode();	//等价于.split()
    
```

