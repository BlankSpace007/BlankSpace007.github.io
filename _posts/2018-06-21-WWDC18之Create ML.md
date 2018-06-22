---
layout: post
title:  "WWDC2018之Create ML"
date:   2018-06-21 21:52:25
categories: mediator feature
tags: featured
image: /assets/article_images/2016-01-15/pic.jpg
---
WWDC2018刚刚过去一个不到一个月，开发者们是不是对iOS12的新特性蠢蠢欲试，在WWDC2018上，库老板发布了iOS12，watchOS 5，macOS Mojave和新Apple TV。iOS12升级了ARKit2，CoreML2，新增了Create ML。那么，我们一定很好奇新增的这个Create ML是什么？和CoreML又有什么关系，下面我带大家看一下。
### **Create ML是什么？**
如果你用过CoreML的话，你应该知道，使用CoreML的前提是你需要机器学习已经训练好的模型，这个模型怎么来呢？使用tensorflow等神经网络框架训练出来，然后转换成相应的格式才能使用，但是这样的话，你需要学习相应的神经网络算法，写相应的神经算法去训练模型，这样学习成本成吨的上涨。这个时候，Create ML出现了，能够让你更加轻松的训练出模型。
### **如何去做？**
下面我将一步一步的演示最基本的做法！  
我们都知道CoreML主要用于识别图像和自然语言，那么，我们就分别以这两个场景来实践。
##### **环境**
电脑系统：macOS Mojave
Xcode版本：Xcode 10 beta2
开发语言：Swift4.2
##### **制作图像分类器**
目标：分类猫和狗的图片
1.数据预处理
首先我们要收集猫和狗的不重复图片数据，里面的80%的图片当做训练集，20%的图片当做测试集。  
新建一个叫Training Data的文件夹，这个文件夹当做训练集的总文件夹，再这个文件夹再新建要分类的文件夹。  
新建一个叫Testing Data的文件夹，这个文件夹当做测试集的总文件夹，和Training Data一样，里面再新建分类的子文件夹。  
总分类文件夹下的每个子分类的图片张数必须要一样，每个子分类的图片至少10张，图片的数目越大，学习后的精确度越高，学习的时间也越长。这些图像不必是相同的大小，也不必是任何特定的大小，尽管最好使用至少299x299像素的图像。图片格式用比较常用的格式都可以，例如JPEG and PNG都可以。  
下面我将使用总共50张猫的图片和50张狗的图片，Training Data下cat和dog的文件夹下的图片数目分别是40张，Testing Data下的cat和dog的文件夹下的图片数目分别是10张.

2.新建一个macOS的Playground文件，注意这里必须选择macOS类型的，如果是iOS或者tvOS的话是无法导入Create ML库的。
![](/assets/article_images/2018-06-21/pic1.png)
3.新建好之后可以输入创建图片分类器UI的代码
![](/assets/article_images/2018-06-21/pic2.png)
这个时候我们再最右面看到图片分类器的Live View。后面的所有操作都是根据这个来操作的。
4.开始操作
展开Live View的ImageClassifier，我们可以看到好多陌生名词，我们一一解释下
![](/assets/article_images/2018-06-21/pic5.png)
**Max iterations**:迭代次数。默认是10，越大训练的时间越长，但是过大的话会造成过拟合。
**Augmentation**下面有4个选项，这4个选项是为了通过**Flip**（翻转），**Rotate**（旋转），**Expose**（曝光），**Shear**（裁剪）四种方式对同一张图片进行处理，这样的话，一张图片经过这四种处理，就变成五张图片。更有利于模型学习，这种技术叫做数据增强。一般情况下，数据不足的情况下会用这种方式来提高准确性。
![](/assets/article_images/2018-06-21/pic6.png)
这里有一个bug，在勾选Augmentation的时候训练时，Live View会消失，可能是数据增强之后数据集变大的原因，希望苹果公司后期会修复这个bug。
下面开始训练：可以将Training Data文件夹拖到虚框里或者在上面的Training Data的Choose..选择Training Data文件夹。这时候，数据就会开始训练，因为我这里迭代次数默认是10，数据集也不多，所以，很快就训练完成了，训练结果如下。
![](/assets/article_images/2018-06-21/pic7.png)
这里会有人疑问，为什么训练也会有正确率？  
答案是作为训练过程的一部分，图像分类器会自动将训练数据分割成训练部分和验证部分，这两者都会影响训练，但方式不同。由于分割是随机进行的，所以每次训练模型时，您可能会得到不同的结果。  
在这里，我们只有分类2种类型，是很简单的场景，所以正确率为100%。
然后，再将Testing Data文件夹拖进去。得到结果：
![](/assets/article_images/2018-06-21/pic8.png)
上面显示测试集预测的准确度为100%，下面是每张测试数据的图片的测试结果。
最后只要把模型保存下来就够了。
![](/assets/article_images/2018-06-21/pic9.png)
填写Author，Description，License，Version，选好保存的地址，点一下Save按钮就保存下来了。
流程就是这样了，大家可以去做一些更加复杂的图片分类器去！
##### **制作语言分类器**






