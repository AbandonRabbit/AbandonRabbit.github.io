---
date: '2025-06-04T18:01:37+08:00'
draft: false
title: '异常'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 7  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---


## Java 异常体系结构

Java 中的异常体系源于 `Throwable` 类，它有两个主要子类：

1. `Error` （错误）：表示 系统级别的严重问题，一般程序无法处理。
2. `Exception` （异常）：表示 程序可以捕获并处理的问题。

### 异常分类
#### Error（错误）
JVM 或系统级别的严重问题，通常由 JVM 抛出，不需要捕获或处理，多是 JVM 本身的问题或者硬件问题。

**常见的错误有**

- OutOfMemoryError（内存不足）
- StackOverflowError（栈溢出）
- NoClassDefFoundError（类加载失败）

#### Exception（异常）

**编译时异常 `Checked Exception`**

编译阶段必须处理、**必须**通过 `throws` 关键字声明异常类型, 标注该方法可能产生的异常类型。

~~~java
public static int function() throws Exception{
  
}
~~~

常见的异常有:

1. IOException（文件读写异常）
2. SQLException（数据库操作异常）
3. ClassNotFoundException（类未找到）

**运行时异常 `RuntimeException`**

不强制处理（可选择性捕获）。通常由编程错误引发（如空指针、数组越界）。

## 自定义异常

继承 `Exception` 或者 `RuntimeException` ，重写带String类型参数的构造。 

~~~java
public class ArrayIsNullOrLengthIsZeroException extends RuntimeException {
    public ArrayIsNullOrLengthIsZeroException() {
    }

    public ArrayIsNullOrLengthIsZeroException(String message) {
        super(message);
    }
}
~~~

## 异常处理

抛出异常
~~~java
throw new 异常对象;
~~~

处理异常
~~~java
try {
    // 可能抛出异常的代码
} catch (IOException e) {
    // 处理 IOException
} catch (Exception e) {
    // 处理其他异常
} finally {
    // 无论是否异常都会执行（常用于释放资源）
}
~~~

**执行流程**：  
首先执行try里面的代码, 一旦出现异常后,try剩下的代码将不会在执行,  就会直接找catch匹配异常, 一旦匹配成功, 执行catch中的异常处理方案, 整个try catch结束。

{{<alert>}}
异常类型一旦有继承关系, 父类异常放到最下面。
{{</alert>}}