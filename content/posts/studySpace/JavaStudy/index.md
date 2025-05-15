---
date: '2025-05-14T09:08:14+08:00'
draft: false
title: 'Java基础'
seriesOpened: false #s是否开启系列
# series: [""] #属于的系列 
# series_order: 0  #系列编号
showSummary: ["Java基础"] #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: false #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---


运行命令  
~~~
java <文件名>
~~~

编译命令  
~~~
javac <文件名>
~~~


新建目录：
工程 -> 模块 -> 包 -> 类

模块名称一般为域名倒写


## 基本数据类型

整数：byte(一个字节)、short(两个字节)、int（四个字节）、long（八个字节）  
小数：floot（四个字节）、double（八个字节）
字符：char（两个字节）
布尔：boolean（一个字节）

**方法定义格式**

~~~
修饰符 返回值类型 方法名（参数列表）{
    方法体；
    return 返回值；
}
~~~