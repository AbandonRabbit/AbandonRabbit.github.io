---
date: '2025-06-04T17:54:03+08:00'
draft: false
title: 'Lambda表达式和方法引用'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 6  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

## Lambda 表达式

**匿名内部类函数的语法糖**，用于简化函数式接口（`@FunctionalInterface`）的实现。

编译后生成静态方法或动态调用点（依赖 `invokedynamic` 指令）。

有且仅有一个抽象方法的接口叫做函数式接口。

*可以通过注解 `@FunctionalInterface` 检查一个接口是不是函数式接口。*  

### 核心语法
~~~java
(参数列表) -> { 方法体 }  
~~~

简化格式：
1. 参数的数据类型可以省略。
2. 当参数有且只有一个的时候 `()` 可以省略。
3. 当方法体只有一行的时候 `{}` 可以省略, `{}` 一旦省略 `return` 和 `;` 都要省略。

## 方法引用

简化lambda的书写, lambda简化函数式接口的匿名内部类的书写, 匿名内部类快速创建子类。

> *就像指向一个方法并说，“需要的时候用这个！”*  
> 我自己的理解就是引用某个方法的方法体，而 Lambda 是定义抽象方法的方法体。  
> 参数和返回值相同才能写方法引用！！！

{{<alert>}}
语法糖的语法糖
{{</alert>}}

进一步简化 Lambda，四种形式：

1. 静态方法引用：类名::静态方法名
    ~~~java
    Function<String, Integer> parser = Integer::parseInt;
    ~~~

2. 实例方法引用：对象::方法名
    ~~~java
    List<String> list = Arrays.asList("a", "b");
    list.forEach(System.out::println);
    ~~~

3. 类构造方法引用：类名::new
    ~~~java
    Supplier<List<String>> listSupplier = ArrayList::new;
    ~~~

4. 任意对象方法引用：类名::实例方法
    ~~~java
    Arrays.sort(strArray, String::compareToIgnoreCase);
    ~~~