---
layout: post
title:  在Keil_v5 for ARM中同时编译ARM和C51程序
date:   '2017-03-28'
categories: [ARM]
---

最近做毕业设计需要用到Keil进行C51编程，此前很久涉及到C51的编程了，一直用的是`Keil_v5.22 for ARM`，可以打开C51工程文件但是无法进行编译，网上搜了一些方法将C51加入到了Keil的编译及芯片列表中，这里进行一下总结。

- 首先下载`Keil_v5 for ARM`和`Keil_v4 for C51`，这里还需要注册码生成破解软件；

- 安装`Keil_v5`到`E:\Keil v5`或其他位置，安装完成后使用注册码生成破解软件对Keil进行破解；

- 安装`Keil_v4`到`E:\Keil_v4`或其他位置，两个软件不要同时安装在同一个文件夹，安装完成后再进行破解；

- 如果破解不成功，尝试右键软件以管理员方式运行，**进行下一步之前确认以上两个软件正常破解**；

- 将`E:\Keil_v4\UV4`下文件全部拷贝到`E:\Keil_v5\UV4`下，如果提示是否覆盖重复文件，点击否；

- 将`E:\Keil_v4\C51`文件夹拷贝到`E:\Keil_v5`文件夹下；

- 使用文本软件打开`E:\Keil_v4\TOOLS.INI`，拷贝所有内容到`E:\Keil_v5\TOOL.INI`的最后一行。

至此所有工作完成，可以下载`Keil_v4 for C51`，使用`Keil_v5 for ARM`可以打开C51工程并能进行编译。