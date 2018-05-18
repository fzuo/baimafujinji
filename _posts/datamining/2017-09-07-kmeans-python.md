---
layout: post
title: k-means聚类及其Python实例
category: 数据挖掘
tags: Data mining
keywords: k-means,数据挖掘
---

> 在2006年12月召开的 IEEE 数据挖掘国际会议上（ICDM， International Conference on Data Mining），与会的各位专家选出了当时的十大“数据挖掘算法”（ top 10 data mining algorithms ）， k-means算法即位列其中。

该算法思路简洁，但是在实践中却相当有效。如果你对其算法原理仍不甚了解，你可以参考本博客之前的文章《K-means算法原理与R语言实例》。在此前的文章中，我们给出的实例是基于R语言实现的。本文将演示在Python语言中利用scikit-learn提供的函数来进行基于 k-means之机器学习的实例。最后，本文还会演示 k-means算法在图像处理中的一个重要应用。

---------------------------------------------------
### 一、参数与方法

scikit-learn中提供的用于进行k-means机器学习的类是sklearn.cluster.KMeans，它所涉及的参数有超过10个之多，但是最常用的其实就是n_clusters 和random_state。其中，前者表示你打算聚类的数目，默认情况下是8。后者表示产生随机数的方法。默认情况下的缺省值为None，此时的随机数产生器是np.random所使用的RandomState实例。你也可以自己指定一个RandomState实例类型作为参数。或者你也可以使用一个整数来作为参数，此时这个整数表示产生随机数的种子。

比较常用的方法包括：

- fit(X[, y])       Compute k-means clustering.
- predict(X)     Predict the closest cluster each sample in X belongs to.
- fit_predict(X[, y])     Compute cluster centers and predict cluster index for each sample.

关于参数和方法的更多详细内容，你可以参考scikit-learn的文档（见文献【1】[^1]），下面我们将主要通过例子来演示它们的使用方法。


---------------------------------------------------
### 二、数据聚类实例

这里所使用的dataset仍然是《K-means算法原理与R语言实例》中的第一张表里所给出的一组二维数据点。

首先引入各种必要的package。

```python
>>> from sklearn.cluster import KMeans
>>> import numpy as np
>>> import matplotlib.pyplot as plt
```

读入数据点（此处数据不是很多，我们遂手动输入）。

```python
>>> X = np.array([[15, 17], [12,18], [14,15], [13,16], [12,15], [16,12],
	      [4,6], [5,8], [5,3], [7,4], [7,2], [6,5]])
```

接下来进行*k*-means聚类，random seed我们选择0，聚类的数目为2。

```python
>>> y_pred = KMeans(n_clusters=2, random_state=0).fit_predict(X)
```

然后来通过图形化的手段展示一下聚类的效果，注意我们把其中一类着成红色，另外一类则着成绿色。

```python
>>> plt.figure(figsize=(20, 20))
>>> color = ("red", "green")
>>> colors=np.array(color)[y_pred]
>>> plt.scatter(X[:, 0], X[:, 1], c=colors)
>>> plt.show()
```

方法fit\_predict的作用是计算聚类中心，并为输入的数据加上分类标签。下面我们再演示fit方法的使用，它仅仅产生聚类中心（其实也就是建模），然后我们引入两个新的点，并利用已经建立的模型预测它们的分类情况。

```python
>>> kmeans = KMeans(n_clusters=2, random_state=0).fit(X)

>>> new_data = np.array([[3,3],[15,15]])
>>> colors2 = np.array(color)[kmeans.predict([[3,3],[15,15]])]

>>> plt.scatter(new_data[:, 0], new_data[:, 1], c=colors2, marker='x')
>>> plt.show()
```

最后来看看聚类的效果，如下图所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining05.png" width="400">
</p>

---------------------------------------------------
### 三、图像处理实例

这是scikit-learn文档中给出的一个实例[^2]（为了便于理解，代码笔者略有精简）。现在要解决的问题是图像处理中的色彩量化（Color Quantization）问题。如下图所示是一张彩色照片，它的色彩是非常丰富的，经统计其中共有96615种不同的色彩。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining06.png" width="450">
</p>

现在我们希望用更少的色彩来展示这张图片（从而实现类似图像压缩的目的），例如之用64种颜色！显然，k-means可以帮助我们将原来的96615种色彩聚合成64个类，然后我们可将新的64个色彩中心作为新图像中所使用的色彩。这其实是类似实现了调色板的功能，如果读者对图像格式比较了解的话，可知很多图像格式中都有调色板的存在。

不仅如此，为了加速聚类过程，其实也并不需要让原来的96615种色彩都参与计算，我们可以随机从中选取部分（例如1000种）颜色来进行计算。

我们将要Jupyter中执行下面的代码。注意在执行下面的代码之前，你要保证安装Pillow包以使得图像显示部分可以执行，否则程序可能会报错！

"Talk is cheap. Show me the code." 同样，首先引入必须要的package。

```python
print(__doc__)
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.metrics import pairwise_distances_argmin
from sklearn.datasets import load_sample_image
from sklearn.utils import shuffle
```

接下来，定义聚类的数目为64，然后载入一章待处理的图像。把图像书籍转换到[0,1]区间，并转换成向量。

```python
n_colors = 64

china = load_sample_image("china.jpg")

# Convert to floats instead of the default 8 bits integer coding. Dividing by
# 255 is important so that plt.imshow behaves works well on float data (need to
# be in the range [0-1])
china = np.array(china, dtype=np.float64) / 255

# Load Image and transform to a 2D numpy array.
w, h, d = original_shape = tuple(china.shape)
assert d == 3
image_array = np.reshape(china, (w * h, d))
```

然后从图中随机选取1000个点（注意这里的“每个点”都是一个类似 [0.25， 0.10， 0.68] 这样的三元组，表示一个像素的三种色彩分量），并以此为基础进行k-means聚类。

```python
print("Fitting model on a small sub-sample of the data")

##shuffle表示随机排列，[:1000]表示排序后去头1000个像素，所以就相当于随机取了1000个点
image_array_sample = shuffle(image_array, random_state=0)[:1000]
kmeans = KMeans(n_clusters=n_colors, random_state=0).fit(image_array_sample)

# Get labels for all points
print("Predicting color indices on the full image (k-means)")
labels = kmeans.predict(image_array)
```

然后是重建图像的函数。

```python
def recreate_image(codebook, labels, w, h):
    """Recreate the (compressed) image from the code book & labels"""
    d = codebook.shape[1]
    image = np.zeros((w, h, d))
    label_idx = 0
    for i in range(w):
        for j in range(h):
            image[i][j] = codebook[labels[label_idx]]
            label_idx += 1
    return image
```

然后来显示一下我们用64种色彩重建后的图像效果。

```python
ax = plt.axes([0, 0, 1, 1])
plt.axis('off')
plt.title('Quantized image (64 colors, K-Means)')
plt.imshow(recreate_image(kmeans.cluster_centers_, labels, w, h))
plt.show()
```

最后来看看处理后的图像效果，可见，尽管大量减少了色彩使用量，图像在细节上仍然基本保持了原貌。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining07.png" width="450">
</p>


参考文献：

[^1]:[网址链接1](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

[^2]:[网址链接2](http://scikit-learn.org/stable/auto_examples/cluster/plot_color_quantization.html)

<span style="color:blue">**（本文完）**</span>
