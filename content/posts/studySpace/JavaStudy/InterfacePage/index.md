---
date: '2025-06-04T17:46:07+08:00'
draft: false
title: '接口'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 3  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

## 接口

使用 `interface` 关键字声明。

{{<alert>}}
接口是行为的抽象，抽象类是事物的抽象。
{{</alert>}}

~~~java
public interface name{
    //常量
    //抽象方法
}

~~~ 

> 有且仅有一个抽象方法的接口叫做函数式接口.

**Java 7 之前**

- 只能定义 `public abstract` 方法。
- 所有方法默认必须被实现类重写，否则实现类也得声明为抽象类。
- 类只能单继承，但能实现多个接口。
- 接口里的字段默认都是 `public static final` ，拿来当全局常量用（但别这么干，枚举更香）。

**Java 8**

- 用 `default` 关键字给接口塞具体实现，打破了纯抽象原则。
- 可以使用 `static` 修饰方法，可以直接通过接口调用。
- 只有一个抽象方法的接口，能当 [Lamdba](#lamdbaText) 用。

> 当多个接口的 `default` 修饰的方法冲突之后，实现的类必须重写这个方法。

**Java 9**
- 引入私有方法，抽取默认方法或者静态方法的共性内容，接口自己使用。

{{<alert>}}
md 这玩意儿怎么越来越像抽象类了。
{{</alert>}}

### 接口和抽象类的区别

1. 接口和类的关系是实现, 而抽象类和类关系是继承。
2. 接口的成员变量是常量, 抽象类成员变量是变量。
3. 接口的灵活性高, 可以让类选择性的实现规则, 而抽象类要求所有的子类都必须实现规则。
4. 接口没有构造方法, 抽象类有构造方法。

### 函数式接口

是指**只包含一个抽象方法**的接口。它可以被 Lambda 表达式、方法引用、构造方法引用等函数式编程方式使用。

可以通过 `@FunctionalInterface` 注解校验一个接口是否函数式接口

**函数式接口规则**
1. 是否有且仅有一个抽象方法。
2. 允许存在默认方法（default）、静态方法、从 Object 继承的方法。