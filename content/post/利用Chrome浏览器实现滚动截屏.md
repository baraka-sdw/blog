---
title: 利用Chrome浏览器实现滚动截屏
date: 2018-05-24 09:07:47
categories: ["工具"]
tags: ["工具"]      
featured_image: https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/bing_photos/20180524.jpg
---

<p class="description">每日一句:It’s not about how badly you want something. It’s about what you are capable of! 
光有志向是不够的，重要的是你的能力。 —《疯狂动物城》.</p>

<!-- more -->
### 1.截取整个页面
#### 1.1 打开 Chrome 浏览器，进入需要截图的网站页面
#### 1.2 打开开发者工具，Window下快捷键 Ctrl + Shift + I
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/20180524.png)
#### 1.3 使用快捷键组合来打开命令行，Ctrl + Shift + P (Windows)
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018052401.png)
#### 1.4 在命令行中输入“Screen”，找到“Capture full size screenshot”并回车
在此之后，Chrome会将截图下载到电脑设定的下载区域。
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018052402.png)

### 2.截取局部元素
#### 2.1 点击开发者工具左上角的“选取元素”按钮，在网页中点击要截图的元素
#### 2.2 由于HTML父子元素的嵌套,可能选中的是需要截图元素的子元素,这时，需要在开发者工具中对所选取的元素进行调整：由于选取的是子元素,
所以只需要在“选取元素”按钮，旁边的"Elements Tab"里边按照嵌套关系，找到合适的父元素就可以了。这时，点击选中该父元素。
#### 2.3 打开命令行，进行截图命令（方法和上面第四步类似）。不过需要注意这时在包含 "Screen" 关键字的命令中选取“Capture node screenshot”而非“Capture full size screenshot”。

扩展：利用 Chrome 的开发者工具，还可以实现对不同型号手机整个页面的截图
点击开发者工具左上角的视图转换按钮(Ctrl + Shift + M)，这时浏览器中的页面会呈现出手机视图。同时，在浏览器中还可以选择不同的
的手机或者平板型号来对比不同硬件上观看页面的不同效果,重新加载页面,打开命令行，进行截图命令
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018052403.png)











