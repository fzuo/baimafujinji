---
layout: post
title: 跟我一起学Keras之实例篇(2)
category: 深度学习
tags: Keras
keywords: Keras,深度学习
---


这次我们要执行的任务和《 基于Softmax实现手写数字识别 》中所描述的基本一致，也就是设法对MINST数据集中的手写数字图片进行识别。如果你通过阅读之前的文章已经对当前问题有所了解，那么也应该知道这其实是一个分类任务，也就是将每张图片分入0~9这个十个类别中。当然首先还是导入各种所需的package以及数据集：

```python
import numpy as np
import random
import keras
import matplotlib.pyplot as plt
%matplotlib inline

from keras.datasets import mnist
from keras.models import Sequential, Model
from keras.layers.core import Dense, Dropout, Activation
from keras.optimizers import RMSprop
from keras.utils import np_utils

(X_train, y_train), (X_test, y_test) = mnist.load_data()

print(X_train.shape, y_train.shape)
print(X_test.shape, y_test.shape)
```

上述程序的输出如下：

```
(60000, 28, 28) (60000,)
(10000, 28, 28) (10000,)
```

其中，60000是训练数据集中图像之数量，10000是测试数据集中图像之数量，28×28是图片的大小。

接下来，要对输入的数据做一些预处理。将每个二维的图像矩阵转换成一个一维的向量，然后再将像素值做归一化，也就是从0~255的取值压缩到0~1之间。

```python
X_train = X_train.reshape(X_train.shape[0], -1) # 等价于X_train = X_train.reshape(60000,784)  
X_test = X_test.reshape(X_test.shape[0], -1)    # 等价于X_test = X_test.reshape(10000,784)  
X_train = X_train.astype("float32")  
X_test = X_test.astype("float32")  
X_train /= 255  
X_test /= 255  
```

然后再对标签数据也做一些处理。下面这个函数的意思是把原来0~9这样的标签，变成长度为10的one-hot向量表示。

```python
y_train = np_utils.to_categorical(y_train, num_classes=10)  
y_test = np_utils.to_categorical(y_test, num_classes=10)  
```

接下来我们便可以开始构建NN了，回忆《跟我一起学Keras之实例篇(1) 》，第一步是定义模型：

```python
model = Sequential()  
model.add(Dense(512, input_shape=(784,)))  
model.add(Activation('relu'))  
model.add(Dropout(0.2))  
model.add(Dense(512))  
model.add(Activation('relu'))  
model.add(Dropout(0.2))  
model.add(Dense(10))  
model.add(Activation('softmax'))  
```

对于已经定义好的模型，你也可以用下面的语句来展示或者总结一下：

```python
model.summary()  
```

上述代码的输出结果如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/keras/keras03.png" width="560">
</p>

接下来就可以进入第二步，即Model compilation。同样跟之前用过的方法一致，其中metrics表示你希望Keras在优化过程中同时计算的一些量：

```python
model.compile(loss = 'categorical_crossentropy',  
             optimizer = 'rmsprop', #RMSprop()  
             metrics=['accuracy'])  
```

此外，之前也曾经讲过，你可以使用默认参数的优化器，那么你只需要传递一个字符串即可。而你也可以自定义优化器中的参数，例如：

```python
rmsprop = RMSprop(lr=0.001, rho=0.9, epsilon=1e-08, decay=0.0)  
  
# metrics means you want to get more results during the training process  
model.compile(optimizer=rmsprop,  
              loss='categorical_crossentropy',  
              metrics=['accuracy'])  
```

然后就可以进入模型训练阶段了。




```
{'batch_size': 128,
 'do_validation': True,
 'epochs': 10,
 'metrics': ['loss', 'acc', 'val_loss', 'val_acc'],
 'samples': 60000,
 'steps': None,
 'verbose': 1}
 ```



接下来看看模型预测的结果如何：

```python
pred = model.predict(X_test_0[:])  
print('Label of testing sample', np.argmax(y_test_0))  
print('Output of the softmax layer', pred[0])  
print('Network prediction:', np.argmax([pred[0]]))  
```

程序的输出如下，可见模型的label是7，经过softmax后的输出向量中7位置的概率也最大，因此神经网络也据此预测出了正确的结果为7。

```
Label of testing sample 7
Output of the softmax layer [  8.34779236e-24   8.63253248e-20   4.66306196e-16   7.04745600e-16
   2.07757194e-25   1.96880282e-22   4.68332635e-33   1.00000000e+00
   4.69165211e-21   4.72258688e-15]
Network prediction: 7
```

--------------------
### 参考文献：

【1】这个例子主要取材自[网址链接](https://www.youtube.com/watch?v=OUMDUq5OJLg)<br>
【2】你可以从[链接](https://pan.baidu.com/s/1o8E96Uu)中获取到本文中所用之完整程序代码的jupyter notebook文件


<span style="color:blue">**（本文完）**</span>
