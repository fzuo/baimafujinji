---
layout: post
title: 匿名函数：从多语言视角阐释
category: 编程与开发
tags: 编程语言
keywords: 编程语言,匿名函数
---

在计算机编程中，匿名函数（anonymous function）是指一类无需定义标识符（函数名）的函数，现在的很多编程语言中都提供有支持匿名函数的语法。匿名函数的设想最初是源自阿隆佐·丘奇（Alonzo Church）在1936年提出的lambda演算子，彼时电子计算机还没有诞生，在lambda演算子中所有的函数都是匿名的。由此，后来引入匿名函数的很多编程语言都用 lambda 作为表示匿名函数语法的关键词。最新引入匿名函数特性的编程语言是Lisp，那是在1958年。LISP是最早出现的一种函数式程序设计语言。我们今天会从多个语言的视角来看看其中的匿名函数语法规则是怎样的。

## Haskell

Haskell 是当前最为流行的一种函数式编程语言。Haskell 语法里有一种叫做高阶函数（Higher order functions）的设计，这有点像C语言中指向函数的指针。

在Haskell中，一阶值（First order values）指的是普通数据。二阶值（Second order values）是指那些以一阶值作为参数和返回结果的函数。三阶值（Third order values）是指那些以二阶值作为参数和返回结果的函数。以此类推，n阶值（*n*th order values）are functions whose arguments and results are values of any order from first up to n − 1。

在Haskell中使用高阶函数时，只要将高阶参数和返回值的类型用括号包裹起来，并以此告诉Haskell这是一个参数即可。例如Haskell prelude已经定义好了的filter函数，我们也可以写出我们自己的版本如下：


```haskell
myfilter :: (a -> Bool) -> [a] -> [a]  
myfilter _ [] = []  
myfilter f (x:xs) =  
    if f x then x:fxs else fxs  
    where fxs = myfilter f xs  
```

然后我们再定义三个函数，分别用来判断一个数是否是偶数，一个数是否是整数，以及一个字符串是否长度超过3：

```haskell
is_even :: Int -> Bool  
is_even x = if (mod x 2) == 0 then True else False  
  
is_pos :: Int -> Bool  
is_pos x = if x > 0 then True else False  
  
is_long :: String -> Bool  
is_long x = if length x > 3 then True else False  
```

下面我们就把上述三个小函数作为参数传递给myfilter函数，并测试一下我们的函数是否能按照既定规则来完成数据筛选：

```haskell
*Main> myfilter is_even [1, 2, 3, 4]  
[2,4]  
*Main> myfilter is_pos [0, -1, 1, -2, 2]  
[1,2]  
*Main> filter is_long ["a", "abc", "abcde"]  
["abcde"]  
```

可见我们的高阶函数执行的很好。


在某些时候, the only thing you need a function for is to pass as an argument to a higher order function like filter. 在这些情况下，readers may find it more convenient if the call contained the definition of the function, not its name. 此时我们便需要使用匿名函数。

在Haskell中, 匿名函数 are defined by lambda expressions, 你可以像下面这样来使用它：

```haskell
filter (\x -> (mod x 2) == 0) [1, 2, 3, 4]  
filter (\s -> length s > 3) ["a", "abc", "abcde"]  
```

This notation is based on the lambda 演算子, the basis of functional programming. （之所以使用\符号，是因为它看起来很像是λ）在lambda 演算子中, each argument is preceded by a lambda, and the argument list is followed by a dot and the expression that is the function body. For example, the function that adds together its two arguments is written as λa.λb.a + b. 这也即是本文最开始讲到的匿名函数的由来。尽管在现代的编程语言中，我们其实并不比去关心lambda 演算子为何物。

## Python
对象当成字符串时的回应方法。例如使用`echo $obj;`来输出一个对象

```php
<?php
// Declare a simple class
class TestClass
{
    public function __toString() {
        return 'this is a object';
    }
}

$class = new TestClass();
echo $class;
?>
```

这个方法只能返回字符串，而且不可以在这个方法中抛出异常，否则会出现致命错误。

### `__invoke()`
调用函数的方式调用一个对象时的回应方法。如下

```php
<?php
class CallableClass 
{
    function __invoke() {
        echo 'this is a object';
    }
}
$obj = new CallableClass;
var_dump(is_callable($obj));
?>
```

### `__set_state()`
调用`var_export()`导出类时，此静态方法会被调用。

```php
<?php
class A
{
    public $var1;
    public $var2;

    public static function __set_state ($an_array) {
        $obj = new A;
        $obj->var1 = $an_array['var1'];
        $obj->var2 = $an_array['var2'];
        return $obj;
    }
}

$a = new A;
$a->var1 = 5;
$a->var2 = 'foo';
var_dump(var_export($a));
?>
```

匿名函数是一种风格问题。不一定非要使用它们；任何能够使用它们的地方，都可以定义一个单独的普通函数来进行替换。我将它们用在需要封装特殊的、非重用代码上，避免令我的代码充斥着大量单行函数。

## C++


这里有一个例子, 把匿名函数存储在变量,数组或vector中,并把它们当做命名参数来传递

```c++
#include<functional>  
#include<vector>  
#include<iostream>  
double eval(std::function<double(double)> f, double x = 2.0){return f(x);}  
int main()  
{  
     std::function<double(double)> f0    = [](double x){return 1;};  
     auto                          f1    = [](double x){return x;};  
     decltype(f0)                  fa[3] = {f0,f1,[](double x){return x*x;}};  
     std::vector<decltype(f0)>     fv    = {f0,f1};  
     fv.push_back                  ([](double x){return x*x;});  
     for(int i=0;i<fv.size();i++)  std::cout << fv[i](2.0) << "\n";  
     for(int i=0;i<3;i++)          std::cout << fa[i](2.0) << "\n";  
     for(auto &f : fv)             std::cout << f(2.0) << "\n";  
     for(auto &f : fa)             std::cout << f(2.0) << "\n";  
     std::cout << eval(f0) << "\n";  
     std::cout << eval(f1) << "\n";  
     return 0;  
}  
```

一个没有指定任何捕获的lambda函数,可以显式转换成一个具有相同声明形式函数指针.所以,像下面这样做是合法的:

```c++
auto a_lambda_func = [](int x) { /*...*/ };  
void(*func_ptr)(int) = a_lambda_func;  
func_ptr(4); //calls the lambda.  
```

> 注：C++的部分引用自[博客文章](http://blog.csdn.net/augusdi/article/details/11773943)，笔者略有修改。

<span style="color:blue">**（本文完）**</span>
