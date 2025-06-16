---
date: '2025-06-16T17:55:02+08:00'
draft: false
title: '时间处理、注解'
series: ["java学习笔记"] #属于的系列 
series_order: 14  #系列编号
showSummary: true #是否显示摘要
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

## 时间

在 java.TIme 包下

### 主要的类
| 类名                | 主要功能                   |
| ------------------- | -------------------------- |
| `Instant`           | 时间戳(精确到纳秒)         |
| `LocalDate`         | 不含时区的日期             |
| `LocalTime`         | 不含时区的时间             |
| `LocalDateTime`     | 不含时区的日期时间         |
| `ZonedDateTime`     | 带时区的日期时间           |
| `Period`            | 日期之间的间隔(年月日)     |
| `Duration`          | 时间之间的间隔(时分秒纳秒) |
| `DateTimeFormatter` | 日期时间格式化             |

**常用方法**

| 方法名    | 描述         |
| --------- | ------------ |
| `now()`   | 当前时间     |
| `of()`    | 指定当前时间 |
| `parse()` | 格式化时间   |

## 注解

以@符号开头的特殊标记，可以附加在类、方法、字段、参数等Java元素上，用于提供额外的信息。

### 定义注解

~~~java
public @interface 注解名{
    常量;
    抽象方法 <default 默认值>;
}
~~~

抽象方法不能有参数，抽象方法的返回值只能为以下类型
> 基本类型：int, short....  
> String  
> Class  
> Enum  
> 注解  

### 元注解

用于定义其他注解的注解：
1. `@Target(ElementType[])` : 指定注解可以应用的目标(类、方法、字段等)。  
   有效值：
   - `ElementType.TYPE` : 类型
   - `ElementType.FIELD` : 属性
   - `ElementType.METHOD` : 方法
   - `ElementType.CONSTRUCTOR` : 构造方法
2. `@Retention` : 限制注解存在的阶段，
   有效值：
   - `RetentionPolicy.SOURCE` : 标注该注解只存在源码阶段。
   - `RetentionPolicy.CLASS` : 标注该注解存在字节码文件中。
   - `RetentionPolicy.RUNTIME` : 标注该注解运行时期在方法中也有效。

### 获取注解值

~~~java

//获取对象字节码
Class<A> clazz = A.class;

//获取类的注解   获取注解对象
Anno anno = c1.getAnnotation(Anno.class);
//获取注解对象的值
System.out.println(anno.name());

//获取方法上的注解
Method method = c1.getMethod("method");
Anno anno1 = method.getAnnotation(Anno.class);
System.out.println(anno1.name());

//获取属性的 也是通过反射
~~~