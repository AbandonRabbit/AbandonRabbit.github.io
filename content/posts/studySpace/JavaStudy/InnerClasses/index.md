---
date: '2025-06-04T17:50:34+08:00'
draft: false
title: '内部类'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 4  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---
## 内部类

定义在类内部的类，可以直接访问外部类的成员（包括 `private` ），反之不行。

编译后会生成独立 `.class` 文件（如 `Outer$Inner.class`）。

### 成员内部类

依附于外部类实例（必须先创建 Outer 才能创建 Inner）。

{{<alert>}}
隐含持有外部类引用（内存泄漏风险点！）。
{{</alert>}}
~~~Java
class Outer {
    private int x = 10;

    private String name;

    public void print() {
        System.out.println("这时外部方法")
    }

    class Inner {
        private String name;
        void print() {
            System.out.println(x); // 直接访问外部类私有成员
            System.out.println(Outer.this.name); // 访问外部同名成员变量，
            Outer.this.print(); // 调用外部类同名方法
        }
    }
}
~~~

**创建方式：**
~~~Java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner(); // 注意 new 语法
~~~

### 静态内部类

~~~Java
class Outer {
    static class Inner {
        void print() {
            // 不能直接访问外部类非静态成员！
        }
    }
}
~~~

- 静态内部类只能访问外部静态的内容。
- 不依赖外部类实例，可以通过 `new Outer.Inner()`

### 局部内部类
{{<alert>}}
用的很少
{{</alert>}}
作用域仅限于方法内（外界不可见）。
~~~java
class Outer {
    void method() {
        class Inner {  // 定义在方法内
            void print() {
                System.out.println("局部内部类");
            }
        }
        Inner inner = new Inner();
        inner.print();
    }
}
~~~

### 匿名内部类

- 本质：创建一个已知类或者已知接口的子类对象。
- 无类名，直接实例化接口或抽象类。
- 匿名内部类会在磁盘中生成 `.class` 文件。

~~~java
new 已知类|已知接口(对参数传递){
    对方法重写
}
~~~
