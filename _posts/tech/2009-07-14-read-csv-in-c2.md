---
layout: post
title: C语言读取CSV文件的方法（下）
category: 编程与开发
tags: C语言
keywords: C语言,CSV文件
---


在之前的文章中，我们已经介绍了利用strok()函数对CSV文件进行解析的基本方法。本文将在此基础上更进一步，我们要将一个用CSV文件存储的表格数据放进一个二维数组中。首先来看看作为示例的一个小型的CSV文件内容：


我们特别约定，CSV文件的第一行的第一个数字表示正式的数据文件一共包含的行数，第二个数字则表示数据文件所包含的列数，注意这里的行数和列数是不包括第一行本身的。



下面我们给出了读取该CSV文件的示例程序：

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    
    char file_name[] = "test_data.csv";
    FILE *fp;
    fp = fopen(file_name, "r");
    
    if (!fp) {
        fprintf(stderr, "failed to open file for reading\n");
        return 1;
    }
    
    
    int idx = 0;
    size_t len = 0;
    char * line = NULL;
    char * element;
    
    getline(&line, &len, fp);
    size_t lines_size = atoi(strtok(line, ","));
    size_t items_size = atoi(strtok(NULL, ","));
    
    double **array;
    array = malloc(lines_size * sizeof(double*));
    if(NULL == array){
        fprintf(stderr, "failed to allocate memory\n");
        return 1;
    }

    while ((getline (&line, &len, fp)) != -1) {
        
        array[idx] = (double*) malloc (sizeof(double) * items_size);
        element = strtok(line, ",");
        for(int j=0; j<items_size; j++){
            array[idx][j] = atof(element);
            element = strtok(NULL, ",");
        }
        idx++;
    }
    
    fclose (fp);
    
    //Test code
    int row = 0;
    for (; row<lines_size; row++) {
        printf ("array[%d][] =", row);
        
        for (int col=0; col<items_size; col++)
            printf (" %f", array[row][col]);
        
        printf("\n");
    }
    
    //free every line
    for (row = 0; row < lines_size; row++)
    {
        free(*(array + row));
        array[row] = NULL;
    }
    
    free(array);
    array = NULL;

    return 0;
}
```

上述代码中特别值得关注的地方有两个：1）二维数组的动态创建和释放；2）我们在读取文件是用到了getline()函数。这是早期版本的C语言中并没包含的一个函数（彼时我们一般用fgets()来代替）。The latest and most trendy function for reading a string of text is getline(). It’s a new C library function, having appeared around 2010 or so. 该函数的原型如下：


```c
getline(&buffer,&size,stdin);  
```

最后执行上述程序，其输出结果如下：

```c
array[0][] = 85.000000 67.000000 34.000000
array[1][] = 27.000000 82.000000 100.000000
array[2][] = 1.000000 23.000000 98.000000
array[3][] = 35.000000 62.000000 78.000000
array[4][] = 21.000000 45.000000 66.000000
```





