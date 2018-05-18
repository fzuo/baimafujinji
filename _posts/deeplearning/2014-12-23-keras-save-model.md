---
layout: post
title: 在Keras中保存已经训练好的模型
category: 深度学习
tags: Keras
keywords: Keras,保持模型
---


之前我们讨论过《在TensorFlow中保存已经训练好的神经网络模型》的方法，彼时我们保存的仅仅是NN中的模型参数，但是在Keras中我们可以非常优雅地把整个模型（包括已经训练好的参数和神经网络的结构）存储起来，而且这一切都“非常非常”简单。

作为一个例子，这里使用《[跟我一起学Keras之实例篇(2)](https://fzuo.github.io/2017/10/30/keras-introduction02.html)》中给出的对MINST数据集进行手写数字识别的代码，并将其中训练好的模型进行保存。

需要提前说明的一点是Keras会把模型保存成“.h5”文件，为了让你的程序可以支持这种形式的文件你需要安装一下h5py这个package，对此你可以在命令行下输入：

```
sudo pip install h5py
```

在你确认完成了上面的步骤之后，接下来的内容就非常简单， 只需要下面这样的语句，已经建立好的模型就会被成功保存了。


```python
from keras.models import load_model

model.save('fei_model.h5')
```

如下图所示，你会看到“fei_model.h5”文件已经生成。完整的包括模型保存部分的手写数字识别代码的jupyter notebook文件可以从文末给出的链接中下载到。

<p align="center">
<img src="https://fzuo.github.io/assets/img/keras/keras06.png" width="580">
</p>

载入已经储存好的模型同样非常简单。你可以另外开一个新文件，导入必要的包和数据（当然无需重新定义和训练模型），然后使用下面这样的语句：

```python
from keras.models import load_model
my_model = load_model('/home/airobot/Desktop/fei_model.h5')
```

被载入的模型是否可以使用呢？我们用它来做一下predict，代码如下：

```python
pred = my_model.predict(X_test_0[:])
print('Label of testing sample', np.argmax(y_test_0))
print('Output of the softmax layer', pred[0])
print('Network prediction:', np.argmax([pred[0]]))

```

如果你有读之前的文章《[跟我一起学Keras之实例篇(2)](https://fzuo.github.io/2017/10/30/keras-introduction02.html)》，可知下面的输出表明，程序准确地预测了手写数字为7。

```
('Label of testing sample', 7)
('Output of the softmax layer', array([  1.55717719e-23,   1.71202164e-13,   5.04616253e-14,
         7.39000111e-11,   3.02617696e-22,   3.59160995e-19,
         9.96468490e-34,   1.00000000e+00,   4.57078810e-19,
         2.00005355e-12], dtype=float32))
('Network prediction:', 7)
```

最后，本文中所使用的源码之jupyter notebook文件均可从[链接](https://pan.baidu.com/s/1nvdxYC5)中获取。


<span style="color:blue">**（本文完）**</span>
