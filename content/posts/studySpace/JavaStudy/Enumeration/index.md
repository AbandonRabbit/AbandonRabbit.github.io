---
date: '2025-06-04T17:43:15+08:00'
draft: false
title: 'Enumeration'
seriesOpened: false #s是否开启系列
series: ["枚举"] #属于的系列 
series_order: 9999  #系列编号
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---
## 枚举

枚举是一个特殊的类，一般表示一组常量，使用 `enum` 关键字来定义，各个常量使用逗号 `,` 来分割。

~~~java
public enum Status {

    UNKNOWN("1"),

    OK("2"),

    ERROR("3");

    private final String value;

    Status(String value) {
        this.value = value;
    }

    public String getValue() {
        return value;
    }
}
~~~

### 构造方法

- 构造方法名和枚举名要保证一致。
- 枚举的构造方法必须是 `private` 的（默认不写也是 `private`），且不能手动调用！
- 线程安全的，但内部字段如果是可变对象。则需要自己加锁。

### 附加信息

通过 **自定义字段** + **构造方法** 将数据和枚举绑定在一起

附加信息为基础类型
~~~java
public enum Status {
    SUCCESS(200, "OK"),
    NOT_FOUND(404, "资源不存在"),
    SERVER_ERROR(500, "服务器炸了");

    private final int code;  // 附加字段1
    private final String msg; // 附加字段2

    // 构造方法（默认private，不写也是）
    Status(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    // 想怎么用就怎么用
    public int getCode() { return code; }
    public String getMsg() { return msg; }
}
~~~

附加信息为对象
~~~java

public enum Book {
    // 枚举常量 + 关联的BookInfo对象
    JAVA("《Java核心技术》", new BookInfo("Cay S. Horstmann", 99.9)),
    PYTHON("《Python编程》", new BookInfo("Eric Matthes", 89.5));

    private final String title;      // 字符串信息
    private final BookInfo info;    // 关联的对象信息

    // 枚举构造函数（自动私有）
    Book(String title, BookInfo info) {
        this.title = title;
        this.info = info;
    }

    // 获取附加对象
    public BookInfo getInfo() {
        return info;
    }

    // 自定义对象类
    public static class BookInfo {
        private final String author;
        private final double price;

        public BookInfo(String author, double price) {
            this.author = author;
            this.price = price;
        }

        // Getters...
    }
}
~~~

调用
~~~java
public class Main {
    public static void main(String[] args) {
        Book javaBook = Book.JAVA;
        System.out.println(javaBook.getInfo().getAuthor()); // 输出: Cay S. Horstmann
        System.out.println(javaBook.getInfo().getPrice());  // 输出: 99.9
    }
}
~~~
