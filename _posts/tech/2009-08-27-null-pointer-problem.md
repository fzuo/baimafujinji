---
layout: post
title: 警惕空指针传递的隐患
category: 编程与开发
tags: 编程
keywords: C语言,编程,空指针
---

## 1. 定义关联模型

在Laravel里面，我们可以通过定义以下Model来完成关联查询。

```php
class MyPost extends Eloquent {
    public function myPostInfo () {
        return $this->hasOne('MyPostInfo');
    }
}

class MyPostInfo extends Eloquent {}
```

## 2. 使用关联模型

这里`myPostInfo()`用的是Camel命名规则，但是我们在读取某一个PostInfo的时候可以用Snake规则。如下面代码都是可行的：

```php
$post = MyPost::find(1);
$post_info = $post->myPostInfo; // example 1
$post_info = $post->my_post_info; // example 2
```

Laravel允许上述两种方法，但是没有合理的处理使用两种命名造成的冲突。

## 3. 缓存失效

如果我们同时使用了上述两个例子，就会使其中一个缓存失效。在Model的relations变量中，缓存了已经读取过的关联Model，但是当我们用不同规则的名字去读取的时候，却会使得前一个缓存失效。例如


```c
int main(int argc, const char * argv[]) {
    
    struct node * head = NULL;
    
    head=(struct node *)malloc(sizeof(struct node));
    head->data = 1;
    head->next = NULL;
    
    if(head == NULL)
        printf("Fail to allocate memory!\n");
    else
        printf("Success!\n");
}
```

所以如果不希望缓存失效，得在项目中只使用一种命名方法去读取关系模型。Laravel推荐的是Camel Case.

执行上述代码，可得输出结果如下：

```
Success! 
```

可见，这样做内存就分配成功了。那产生这种差异的原因又何在呢？原因就在与指针变量在声明之后，首先应当被初始化，然后才能够使用。否则，像我们最开始那样处理的话，函数之间传递的就是一个空指针。

我们应当时刻注意把握指针的本质到底是什么。指针是一个内存地址，而指针变量就是储存这个地址的变量，因为我们的指针变量没有被初始化，所以这个变量中其实就没存地址。于是test()函数进行的内存分配任务在main()函数中都是无效的。但是如果我们就是想在另外一个函数中进行内存分配，而在当前函数中使用这个指针变量所指向的内存，又该如何是好呢？答案就是使用指向指针的指针。本博客之前的文章中我们已经见识了用指向指针的指针来作为二维数组（矩阵）使用的例子，下面这个例子就演示了它的另外一种功用。

```c
void test2(struct node ** head){

    *head=(struct node *)malloc(sizeof(struct node));
    (*head)->data = 1;
    (*head)->next = NULL;

}

int main(int argc, const char * argv[]) {
    
    struct node * head = NULL;
    
    test2(&head);
    
    if(head == NULL)
        printf("Fail to allocate memory!\n");
    else
        printf("Success!\n");
}

```

编译并执行上述代码，可得如下输出结果

```
Success! 
```

可见我们的方法是可行的。


**（本文完）**
