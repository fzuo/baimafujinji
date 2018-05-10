---
layout: post
title: 警惕空指针传递的隐患
category: 编程与开发
tags: 编程
keywords: C语言,编程,空指针
---

C语言中的指针灵活强大，但是要掌握好却相当不易。特别是再配合malloc()函数进行动态内存管理时，很多细节的问题往往较难察觉。本文通过一个例子来探讨C语言中，发生空指针传递时可能会出现的问题。

在下面这个程序中，我们在main()函数里声明一个结构体类型的指针，然后我们在一个函数test()里对它分配内存以及初始化。<span style="color:Orange">注意我们省略了动态内存的回收语句free，在实际编程中应该加上这部分。</span>

```c
struct node{
    int data;
    struct node *next;
};

void test(struct node * head){
    
    head=(struct node *)malloc(sizeof(struct node));
    head->data = 1;
    head->next = NULL;
}

int main(int argc, const char * argv[]) {
    
    struct node * head = NULL;
    
    test(head);
    
    if(head == NULL)
        printf("Fail to allocate memory!\n");
    
    return 0;
}
```

执行上述代码，可得输出结果如下：

```
Fail to allocate memory!
```

可见，内存分配和初始化过程失败了！指针变量head的值还是NULL。

为了查明原委，我们把test()函数中实现的内容挪移回主函数之后来看看效果：


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
