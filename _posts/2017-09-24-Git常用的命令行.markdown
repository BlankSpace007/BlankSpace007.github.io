---
layout: post
title:  "如何让我们愉快的自动化编程(一)"
date:   2016-01-15 14:34:25
categories: mediator feature
tags: featured
image: /assets/article_images/2016-01-15/pic.jpg
---

# 如何让我们愉快的自动化编程(一)
大家是不是还在烦恼每周一的时候去统计网站上统计自己app的数据，最近我们老大就向我们提出一个要求，每周一去友盟统计上去统计上一周的数据。身为极客(其实就是懒)的我，就想着怎么利用自动化在周一的自动运行脚本生成具有上周数据的xls，今天我们先解决如何在Mac上配置定时任务配置。

(一)为什么利用Mac上定时任务配置
为什么要用定时任务配置呢，因为这样我们可以不用去关注何时去运行脚本，甚至根据需求连脚本的参数都不需要输入。具体怎么办，就听我慢慢道来。
(二)跟着我做一个小项目来实验一下
要求：每天的11点定时运行我的脚本，把当前时间写到output.txt文件中，结果output.text里面显示
*  编写任务的脚本
  在这里我用ruby写脚本
 
```ruby
#!/usr/bin/ruby

//获取当前时间
today = Time.new; 
a = "当前日期：" + today.strftime("%Y-%m-%d %H:%M:%S");

//创建或打开当前路径下的文件并写入当前日期
aFile = File.new("/Users/hoping_sir/AutoTask/test1/output.txt", "a")
if aFile
   aFile.syswrite("#{a}\n")
else
   puts "Unable to open file!"
end
```

* 编写任务配置文件
我们需要用到Mac自带的方法，launchctl
然后新建一个plist文件，名叫com.test.plist，里面的基本格式如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.auto.test1</string>
	<key>Program</key>
	<string>/Users/hoping_sir/AutoTask/test1/test1</string>
	<key>StartCalendarInterval</key>
	<dict>
		<key>Minute</key>
		<integer>0</integer>
		<key>Hour</key>
		<integer>11</integer>
	</dict>
</dict>
</plist>
```
以上是这些参数就是我们所需要的，当然配置文件的参数有很多，RunAtLoad就是在加载时候就运行，LaunchOnlyOnce就是只运行一次等等，当然，你想看全部参数，我这边也提供了[传送门](www.baidu.com)。你可以深入研究一下。
我们先好好分析下上面的这些参数代表什么。。。

那么已经完成了任务配置文件，那么我们就要加载这个任务配置文件，说到这个，就要先了解下，Mac上的进程。
如果你进到刚刚的传送门上面的话，你就会发现：

```
FILES
~/Library/LaunchAgents         Per-user agents provided by the user.
/Library/LaunchAgents          Per-user agents provided by the administrator.
/Library/LaunchDaemons         System-wide daemons provided by the administrator.
/System/Library/LaunchAgents   Per-user agents provided by Mac OS X.
/System/Library/LaunchDaemons  System-wide daemons provided by Mac OS X.
```
这代表什么呢？让我来翻译一下！

```
~/Library/LaunchAgents          由用户自己定义的任务项
/Library/LaunchAgents           由管理员为用户定义的任务项
/Library/LaunchDaemons          由管理员定义的守护进程任务项
/System/Library/LaunchAgents    由Mac OS X为用户定义的任务项
/System/Library/LaunchDaemons   由Mac OS X定义的守护进程任务项
```
然而，我们把任务配置文件放到~/Library/LaunchAgents里面就行了。仅仅是放进去是不够的，我们仍需要命令去启动。
* 添加

 ```
  launchctl load ~/Library/LaunchAgents/com.test.plist
 ``` 
 那么怎么看任务有没有加载进去呢？launchctl有自带的方法
 
```
launchctl list
```
 ![](/assets/article_images/2016-01-15/1111111.png)
 我们会发现有好多任务，头疼啊，找起来好麻烦啊，对呀，还有这个方法
 
```
launchtcl list | grep "auto"
```
grep 后面添加的是任务的关键词，这个时候就会出现这么个情况
![](/assets/article_images/2016-01-15/22222.png)
那么这就已经大功告成了
如果你想修改任务里面的参数，就必须要unload

```
 launchctl unload ~/Library/LaunchAgents/com.test.plist
```
等到修改好之后再重新load
那么，你现在是不是感觉有所感悟了。那就心动不如行动，自动化自己的任务吧！！！
 



 


