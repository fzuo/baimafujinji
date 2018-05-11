---
layout: post
title: 在Excel使用VLOOKUP进行列查找
category: 技巧与工具
tags: Excel
keywords: Excel,VLOOKUP
---

VLOOKUP是Excel中一个非常有用的函数。VLOOKUP函数是Excel中的一个纵向查找函数，它与LOOKUP函数和HLOOKUP函数属于一类函数，在工作中都有广泛应用。VLOOKUP是按列查找，最终返回该列所需查询列序所对应的值；与之对应的HLOOKUP是按行查找的。

纵向查找，又可以形象地称为是交叉查找。假设有像下面这样一个实验用数据。如果我们想知道ID=A003的人，physics得了多少分，就可以用VLOOKUP函数。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel10.png" width="320">
</p>

该函数的语法规则为：

```
VLOOKUP(lookup_value,table_array,col_index_num,range_lookup)
```

各参数的意义如下：

- Lookup_value为需要在数据表第一列中进行查找的数值。Lookup_value 可以为数值、引用或文本字符串。当vlookup函数第一参数省略查找值时，表示用0查找。

- Table_array为需要在其中查找数据的数据表。使用对区域或区域名称的引用。

- col_index_num为table_array 中查找数据的数据列序号。col_index_num 为 1 时，返回 table_array 第一列的数值，col_index_num 为 2 时，返回 table_array 第二列的数值，以此类推。注意这里的列数是从引用范围的第一列做为1，而不是以A列作为第一列。如果 col_index_num 小于1，函数 VLOOKUP 返回错误值 #VALUE!；如果 col_index_num 大于 table_array 的列数，函数 VLOOKUP 返回错误值#REF!。

- Range_lookup为一逻辑值，指明函数 VLOOKUP 查找时是精确匹配，还是近似匹配。如果为false或0 ，则返回精确匹配，如果找不到，则返回错误值 #N/A。如果 range_lookup 为TRUE或1，函数 VLOOKUP 将查找近似匹配值，也就是说，如果找不到精确匹配值，则返回小于 lookup_value 的最大数值。如果range_lookup 省略，则默认为近似匹配。

来看下面这个例子。

首先我们在一个单元格中编辑公式，如下图所示，输入“ =VLOOKUP(... ”，其中第一个参数是要查找的内容，你可以输入一个具体的值，例如图中所示的“A003”，或者你也可以用具体某个单元格所在的位置坐标来表示要查找的对象，例如使用A4。

接下来，我们要选中要进行搜索的位置，注意这需要包含作为你直接搜索对象的第一列。然后是用数字表示的将要返回的对应列的偏移（也就是你选择之区域中的位置），最后用一个布尔变量来表示你打算进行精确匹配还是模糊匹配。


最终我们成功地返回了要查找的内容，如下图所示：





**（本文完）**
