---
layout: post
title:  "matplotlib的使用指南"
date:   2017-02-21 21:01:25
categories: mediator feature
tags: featured
image: /assets/article_images/2016-01-15/pic.jpg
---

# matplotlib的使用指南
matplotlib在数据分析挖掘中是十分常见的一个库，它可以将数据图像化。那么，我们今天讲学习下如何这个库的常用用法。

我们先简单做个demo:y=x的图像。
```
import matplotlib.pyplot as plt
import numpy as np 
x=np.linspace(-1,1,50)
y=x+1
plt.plot(x,y)
plt.show()
```
![demo](/assets/article_images/2017-02-21/Figure_demo.png)
是不是很兴奋，一个很简单的图像就出来了。
我大概的解释下代码：

`x=np.linspace(-1,1,50)`
这句话代码是x的区间是（-1，1），平均分成50个点。也就是x轴的范围

`y=x`就是方程式

`plt.plot(x,y)`plot是折线图的方法，里面可以放参数，必须的参数就是x和y，其实还有其他参数，是和样式有关的

`plt.show()`show()方法就是图形展示出来
下面，正式讲解motplotlib的用法。
### **1.Figure**
我们先测试下代码
```
import matplotlib.pyplot as plt
import numpy as np 
x=np.linspace(-1,1,50)
y1=x
y2=x**2
plt.figure() 
plt.plot(x,y1)
plt.figure(num=3)
plt.plot(x,y2)
plt.show()
```
![figure演示](/assets/article_images/2017-02-21/matplotlib_figure.png)
我们可以发现，有两个figure，一个是figure1，一个是figure3。里面的核心代码
`plt.figure()`创建一个figure，如果只有一个的话可以省略。多个的话，必须新建。
`plt.figure(num=3)`新建一个figure3的figure。

### **2.坐标轴**
一般情况下坐标轴上面是数字，那么如果我们要设置文字怎么办呢。
```
import matplotlib.pyplot as plt
import numpy as np 
x=np.linspace(0,100,100)
y=x
plt.plot(x,y)
plt.xlim((0,100))
plt.ylim((0,100))
plt.xlabel(r"x axis")
plt.ylabel(r"y axis")
plt.yticks([60,80],[r"$PASS$",r"$EXCELLENT$"])
plt.show()
```
![坐标轴的图示](/assets/article_images/2017-02-21/Figure_2.png)
我们观察上图可以发现纵坐标变成两个英文。一个是代表60分及格，一个代表80分优秀。
`plt.xlim((0,100))` x轴的范围
`plt.ylim((0,100))` y轴的范围
`plt.xlabel(r"x axis")`x轴的名字
`plt.ylabel(r"y axis")`y轴的名字
`plt.yticks([60,80],[r"$PASS$",r"$EXCELLENT$"])`y轴60，80的位置变成PASS和EXCELLENT

### **3.图例**
```
import matplotlib.pyplot as plt
import numpy as np 
x=np.linspace(0,100,100)
y=x
plt.plot(x,y,label="y=x")
plt.legend()
plt.show()
```
![图例的示例](/assets/article_images/2017-02-21/Figure_3.png)
左上角那个y=x就是图例，可以表达折线的含义。
```
plt.plot(x,y,label="y=x")
plt.legend()
```
在plot方法里面加入一个label的参数，里面就是图例的名字，后面要加legend()方法。

### **4.标注**
除了图例以外，也可以用标注
```
import matplotlib.pyplot as plt
import numpy as np 
x=np.linspace(0,100,100)
y=x
plt.plot(x,y)
x0=50
y0=x0
plt.scatter(x0,y0)
plt.plot([x0,x0],[y0,0],'k--',lw=2.5)
plt.annotate(r"$y=x$", xy=(x0,y0),xycoords="data",xytext=(+30,-30),textcoords="offset points",fontsize=16,arrowprops=dict(arrowstyle='->',connectionstyle='arc3,rad=.2'))
plt.legend()
plt.show()
```
![](/assets/article_images/2017-02-21/Figure_4.png)
还可以用标注的方法。这个是比较麻烦的
里面有两条折线
```plt.plot(x,y)```蓝色的折线
```plt.plot([x0,x0],[y0,0],'k--',lw=2.5) ```黑色虚线
```plt.scatter(x0,y0)```x0,y0的点
```plt.annotate(r"$y=x$", xy=(x0,y0),xycoords="data",xytext=(+30,-30),textcoords="offset points",fontsize=16,arrowprops=dict(arrowstyle='->',connectionstyle='arc3,rad=.2'))```
这个方法里面参数比较多，我来一一分析
```r"$y=x$"```标注的文字
```xy=(x0,y0)```箭头指向的点坐标
```xytext=(+30,-30)```文字偏移位置
```textcoords="offset points"```标注偏移方向
```arrowprops=dict(arrowstyle='->',connectionstyle='arc3,rad=.2'))```箭头的样式和偏转角度

### **5.散点图**
```
n=1024
X=np.random.normal(0,1,n)
Y=np.random.normal(0,1,n)
plt.scatter(X,Y,s=75,alpha=0.5)
plt.xlim((-1.5,1.5))
plt.ylim((-1.5,1.5))
plt.xticks(())
plt.yticks(())
plt.show()
```
![散点图](/assets/article_images/2017-02-21/Figure_5.png)

那么我们来解释下代码
```
n=1024
X=np.random.normal(0,1,n)
Y=np.random.normal(0,1,n)
```
n为1024个点，XY为横纵坐标
```
plt.scatter(X,Y,s=75,alpha=0.5)
```
X,Y是坐标点，s是点的大小，alpha是透明度


### **6.柱状图**
```
n=12
x=np.arange(n)
y=(1-x/float(n))*np.random.uniform(-1,1.0,n)
plt.bar(x,+y,facecolor="#9999ff",edgecolor="black")
for x,y in zip(x,y):
    if(y>0):
        plt.text(x,y+0.05,"%.2f"%y,ha="center",va="bottom")
    else:
        plt.text(x,y-0.05,"%.2f"%y,ha="center",va="top")    
plt.xlim((-0.5,n))
plt.ylim((-1.5,1.5))
plt.xticks(())
plt.yticks(())
plt.show()
```
![](/assets/article_images/2017-02-21/Figure_6.png)

里面的核心代码是
```
plt.bar(x,+y,facecolor="#9999ff",edgecolor="black")
```
bar方法里面facecolor是bar的颜色，edgecolor是边框的颜色。
是不是很简单，当然里面还有其他的参数。大家可以网上查一下


### **7.等高线图**
```
def f(x,y):
    return(1-x/2+x**5+y**3)*np.exp(-x**2-y**2)
n=256
x=np.linspace(-3,3,n)
y=np.linspace(-3,3,n)
X,Y=np.meshgrid(x,y)
plt.contourf(X,Y,f(X,Y),8,alpha=0.75,cmap=plt.cm.cool)
C=plt.contour(X,Y,f(X,Y),8,colors="black",linewidth=.5)
plt.clabel(C,inline=True,fontsize=10)
plt.xticks(())
plt.yticks(())
plt.show()
```
![](/assets/article_images/2017-02-21/Figure_7.png)
是不是很好看呢？我们来理解下代码
```
plt.contourf(X,Y,f(X,Y),8,alpha=0.75,cmap=plt.cm.cool)
```
填充等高线的颜色, 8是等高线分为几部分
```
C=plt.contour(X,Y,f(X,Y),8,colors="black",linewidth=.5)
```
绘制等高线
```
plt.clabel(C,inline=True,fontsize=10)
```
等高线上的数据 

现在看来是不是很清晰了


 


