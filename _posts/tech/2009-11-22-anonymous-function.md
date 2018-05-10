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

在Haskell中，一阶值（First order values）指的是普通数据。二阶值（Second order values）是指那些以一阶值作为参数和返回结果的函数。三阶值（Third order values）是指那些以二阶值作为参数和返回结果的函数。以此类推，n阶值（*n*th order values）are functions whose arguments and results are values of any order from first up to *n* − 1。

在Haskell中使用高阶函数时，只要将高阶参数和返回值的类型用括号包裹起来，并以此告诉Haskell这是一个参数即可。例如Haskell prelude已经定义好了的filter函数，我们也可以写出自己的版本如下：

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

下面我们来看看在Python中如何使用匿名函数。首先，我们定义的一个简单的函数来完成一个数的平方计算：

```python
def f(x):  
    return x*x  
print f(4)
```

如果在Python中使用匿名函数实现的话，则有：

```python
>>> g = lambda x:x*x  
>>> print(g(3))  
9  
>>> print ((lambda x:x*x)(3))  
9  
```

上面的Python代码中并没有专门定义函数，而是使用的匿名函数实现。匿名语句中，冒号前是参数，可以有多个，用逗号隔开，冒号右边的为表达式。其实lambda返回值是一个函数的地址，也就是函数对象。（还是想想C语言中指向函数的指针）


匿名函数可以接收任意多个参数 (包括可选参数) 并且返回单个表达式的值。匿名函数不能包含命令，包含的表达式不能超过一个。不要试图向匿名函数中塞入太多的东西；如果你需要更复杂的东西，应该定义一个普通函数，然后想让它多长就多长。

下面给出一个接受多个参数的匿名函数的例子，同时我们还给出了一个包括可选参数的匿名函数的例子：

```python
>>> add = lambda x, y: x + y  
>>> add(2, 3)  
5  
>>> add2 = lambda x, y = 2: x + y  
>>> add2(1)  
3  
>>> add2(1, 3)  
4  
```

匿名函数是一种风格问题。不一定非要使用它们；任何能够使用它们的地方，都可以定义一个单独的普通函数来进行替换。我将它们用在需要封装特殊的、非重用代码上，避免令我的代码充斥着大量单行函数。

## C++

最后来看看C++中使用匿名函数的做法。注意匿名函数是C++ 11 最新引入的语法规则。C++11提供的对匿名函数的支持，或者称为Lambda函数，其表达式具体形式如下:

- [capture](parameters)->return-type{body}

如果没有参数,空的圆括号()可以省略.返回值也可以省略,如果函数体只由一条return语句组成或返回类型为void的话.形如:

- [capture](parameters){body}

下面来看几个匿名函数的例子：

```c++
[](int x, int y) { return x + y; } // 隐式返回类型
[](int& x) { ++x; }   // 没有return语句 -> lambda 函数的返回类型是'void'
[]() { ++global_x; }  // 没有参数,仅访问某个全局变量
[]{ ++global_x; }     // 与上一个相同,省略了()
```

可以像下面这样显示指定返回类型

```c++
[](int x, int y) -> int { int z = x + y; return z; }
```

在这个例子中创建了一个临时变量z来存储中间值. 和普通函数一样,这个中间值不会保存到下次调用. 什么也不返回的Lambda函数可以省略返回类型, 而不需要使用 -> void 形式。

Lambda函数可以引用在它之外声明的变量. 这些变量的集合叫做一个闭包. 闭包被定义在Lambda表达式声明中的方括号[]内. 这个机制允许这些变量被按值或按引用捕获。下面这些例子就是：

```c++
[]        //未定义变量.试图在Lambda内使用任何外部变量都是错误的.
[x, &y]   //x 按值捕获, y 按引用捕获.
[&]       //用到的任何外部变量都隐式按引用捕获
[=]       //用到的任何外部变量都隐式按值捕获
[&, x]    //x显式地按值捕获. 其它变量按引用捕获
[=, &z]   //z按引用捕获. 其它变量按值捕获
```

接下来的两个例子演示了Lambda表达式的用法：

```c++
std::vector<int> some_list;  
int total = 0;  
for (int i=0;i<5;++i) some_list.push_back(i);  
std::for_each(begin(some_list), end(some_list), [&total](int x)   
{  
    total += x;  
});  
```

此例计算list中所有元素的总和。变量total被存为lambda函数闭包的一部分。因为它是栈变量(局部变量)total的引用，所以可以改变它的值。  

```c++
std::vector<int> some_list;
int total = 0;
int value = 5;
std::for_each(begin(some_list), end(some_list), [&, value, this](int x) 
{
    total += x * value * this->some_func();
});
```

此例中total会存为引用, value则会存一份值拷贝. 对this的捕获比较特殊, 它只能按值捕获. this只有当包含它的最靠近它的函数不是静态成员函数时才能被捕获.对protect和priviate成员来说, 这个lambda函数与创建它的成员函数有相同的访问控制. 如果this被捕获了,不管是显式还隐式的,那么它的类的作用域对Lambda函数就是可见的. 访问this的成员不必使用this->语法,可以直接访问。

不同编译器的具体实现可以有所不同,但期望的结果是:按引用捕获的任何变量,lambda函数实际存储的应该是这些变量在创建这个lambda函数的函数的栈指针,而不是lambda函数本身栈变量的引用. 不管怎样, 因为大数lambda函数都很小且在局部作用中, 与候选的内联函数很类似, 所以按引用捕获的那些变量不需要额外的存储空间。

如果一个闭包含有局部变量的引用,在超出创建它的作用域之外的地方被使用的话,这种行为是未定义的!

lambda函数是一个依赖于实现的函数对象类型，这个类型的名字只有编译器知道。如果用户想把lambda函数做为一个参数来传递, 那么形参的类型必须是模板类型或者必须能创建一个std::function类似的对象去捕获lambda函数。使用 auto关键字可以帮助存储lambda函数。

```c++
auto my_lambda_func = [&](int x) { /*...*/ };
auto my_onheap_lambda_func = new auto([=](int x) { /*...*/ });
```

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
