---
date: '2025-05-14T09:08:14+08:00'
draft: false
title: 'Java基础'
seriesOpened: false #s是否开启系列
# series: [""] #属于的系列 
# series_order: 0  #系列编号
showSummary: true #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
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


新建项目步骤：
工程 -> 模块 -> 包 -> 类

模块名称一般为域名倒写。


## 基本数据类型

整数：byte(一个字节)、short(两个字节)、int（四个字节）、long（八个字节）  

小数：floot（四个字节）、double（八个字节）

字符：char（两个字节）

布尔：boolean（一个字节）

变量定义：

~~~
数据类型 变量名 = 初始值;
~~~

## 方法&运算符

### 方法

~~~
修饰符 返回值类型 方法名(形参列表) {
方法体代码(需要执行的功能代码) 
return 返回值;
}
~~~

*无返回值的方法中可以直接通过单独的return;立即结束当前方法的执行。*

**方法重载**：方法名相同，参数列表不同。

### 类型转换

1. **自动类型转换**类型范围小的变量，可以直接赋值给类型范围大的变量。
2. **强制类型转换**大范围类型的变量直接赋值给小范围类型的变量，需要通过强制类型转换来实现。强制类型转换可能造成数据丢失。
3. **表达式的自动类型提升**小范围类型的变量会自动转换成表达式中较大范围的类型，再参与运算。

### 运算符

基本算数运算符包括+（加）、-（减）、*（乘）、/（除）、%（取余）。

自增（++）和自减（--）运算符

扩展赋值运算符包括+=、-=、*=、/=、%=

关系运算符包括 >（大于）、>=（大于等于）、<（小于）、<=（小于等于）、==（等于）、!=（不等于），用于判断数据是否满足条件，最终返回布尔类型的值（true或者false）。

三元运算符  格式：条件表达式? 值1 : 值2。

逻辑运算符  &（逻辑与）、  &&（短路与）、  |（逻辑或）、  ||（短路或）、  !（逻辑非）、  ^（逻辑异或）。

## 流程控制

### 分支

**if 分支**

~~~
if (条件表达式) {
    代码;
}

if (条件) {
    语句体 1;
} else {
    语句体 2;
}

if (条件 1){
    语句体 1;
} else if (条件 2){
    语句体 2;
} else if (条件 3){
    语句体 3;
}...
 else{
    语句体 n +1;
}
~~~

**switch 分支**

~~~
switch(表达式){
    case 值1:
        语句体1;
        break;
    case 值2:
        语句体1;
        break;
    ...
    default:
        语句体n;
        break;    
}
~~~

### 循环

**for 循环**

~~~
for (初始化语句; 循环条件; 迭代语句) {
    循环体语句(重复执行代码);
}
~~~

**while 循环**

~~~
while (循环条件) {
    循环体语句(重复执行代码);
    迭代语句;
}
~~~


**do - while 循环**

先执行循环体再判断条件，至少执行一次循环体。

~~~
do {
    循环体语句;
    迭代语句;
} 
~~~

**死循环**：常用于服务器程序持续监听、后台任务持续运行场景，但使用需谨慎，确保有合理退出机制，防系统资源耗尽崩溃。如服务器监听客户端连接请求，循环等待并处理连接，同时设心跳检测或管理员指令停止机制。

## 数组

占用连续的存储空间

- 静态初始化
  ~~~
  完整格式：
  数据类型[] 数组名 = new 数据类型 []{元素 1，元素 2，元素 3… }; 

  简化格式：
  数据类型[] 数组名 = {元素 1，元素 2，元素 3，… }; 
  ~~~

- 动态初始化：
  ~~~
  数据类型 [][] 数组名 = new 数据类型 [长度 1][长度 2]; 
  ~~~

获取长度 ```arr.length```

**二维数组**

~~~
完整格式: 
数据类型 [][] 数组名 = new 数据类型 [][]{
 {元素1,元素2 ...},
 {元素2,元素3...},
 {元素4,元素5...}
}; 
简化格式: 
数据类型 [][] 数组名 = {
 {元素1,元素2 ...},
 {元素2,元素3...},
 {元素4,元素5...}
}; 
~~~

## 内存划分
当一个方法开始执行，该方法会被加载到方法区（栈结构，栈顶为正在执行的方法），方法的内部使用 new 关键字初始化的对象会被加载到堆中，堆中开辟出来的对象都带默认值。由 JVM 的垃圾回收器管理内存回收，生命周期不确定，可能长期存在，直到没有引用指向它，一些局部变量会被存储到栈中，栈中主要存储基本数据类型的变量和对象的引用（指针）。随着方法调用入栈，方法结束释放。

## 面向对象入门

**1. 类基本语法及定义**

~~~
public class 类名 {
    //构造器
    //成员变量
    //成员方法
}
~~~

**2. 创建对象**

~~~
类名 对象名 = new 类名();
~~~

**3. 访问对象属性和方法**

~~~
对象名.方法名

对象名。属性名
~~~

**4. 构造器**

构造函数一般用于结合new关键字创建对象时初始化成员变量用的

*构造器主要用来初始化数据到堆中*

类如果没有手动设置构造函数, 会默认自带无参构造器。

如果定义了有参构造器，默认无参构造器会消失，若需要则需手动添加。

~~~
修饰符  类名(参数列表){

}
~~~

> 注意事项:
> 1. 构造函数的名字和类名必须一致
> 2. 构造函数不能返回值类型, 连void都不能有
> 3. 构造函数可以重载

## 面向对象 static 、继承

### static

static是一个修饰符,可以修饰成员变量和成员方法等

**修饰变量**

有static修饰，属于类，在内存中只有一份，被类的所有对象共享。  

访问类变量的格式为: 类名.静态变量（推荐）或对象.静态变量（不推荐）。

**修饰方法**

属于类，访问类方法的格式: 可以直接用类名访问（推荐），也可以用对象访问（不推荐）。

使用 static 修饰的变量或者放啊随着类的加载而被加载到堆的静态区，变量被所有对象共享。

> 静态方法主要用来做工具类，调用方便，代码复用性高

{{<alert>}}
1. 静态方法中可以直接访问静态成员，不可以直接访问实例成员。
2. 实例方法中既可以直接访问静态成员，也可以直接访问实例成员。
3. 实例方法中可以出现this关键字，静态方法中不可以出现this关键字。
4. 使用 static 修饰的成员不能被继承。
{{</alert>}}

## 继承

提高代码重用性，减少重复代码书写。

不能循环继承。

~~~
public class 子类 extends 父类{

} 
~~~

- 子类能继承父类的非私有成员（成员变量、成员方法）。
- 子类的对象由子类和父类共同完成。

```super``` 在子类中访问父类的属性或方法。

~~~
super.父类方法
~~~

### 权限修饰符

限制类中成员（成员变量、成员方法、构造器）的访问范围。

- `private`：只能本类访问。  
  缺省（默认，无修饰符）：本类、同一个包中的类可访问。
- `protected`：本类、同一个包中的类、子孙类中可访问。
- `public`：任意位置可访问。



| 修饰符    | 本类 | 本包不同类 | 不同包的子类 | 不同包的无关类 |
| --------- | ---- | ---------- | ------------ | -------------- |
| public    | √    | √          | √            | √              |
| protected | √    | √          | √            |                |
| 缺省      | √    | √          |              |                |
| private   | √    |            |              |                |

### 继承的特点

1. 就近原则：
在子类方法中访问其他成员，先在子类局部范围找，然后子类成员范围找，接着父类成员范围找，若父类范围找不到则报错。
2. 单继承模式：
Java 中一个类只能继承一个直接父类
3. 多层继承：
Java 不支持多继承，但支持多层继承。
4. 祖宗类：
Java 中所有类要么直接继承 Object ，要么默认继承 Object ，要么间接继承 Object ， Object 是所有类的祖宗类。
5. 子类访问父类成员：
若子父类中出现重名成员，优先使用子类的，若要在子类中使用父类的，
可通过 `super` 关键字指定访问父类成员  
格式为: （`super.父类成员变量/super.父类成员方法`）。

### 方法重写

重写父类的方法，需要方法名、参数列表保证一致。

建议使用@Override注解，可检查重写格式是否正确且提高代码可读性。

- 子类重写父类方法时，访问权限必须大于或等于父类该方法的权限（public > protected > 缺省）。
- 重写的方法返回值类型必须与被重写方法的返回值类型一样或范围更小。
- 私有方法、静态方法不能被重写，否则报错。

### 子类构造器

~~~java
super(value1，value2，...)
~~~

> 在子类的构造器里面使用，必须写在第一行。
> 
> 括号里面可以加参数，会自动匹配相应参数的构造器
> 
> 如果父类没有无参构造器，则子类必须显示使用 super( value... )。


~~~java
this(value1，value2，...)
~~~

> 在当前类的构造器里面使用，调用当前类的构造器，必须卸载第一行。
> 
> 可以加入参数，自动匹配相应参数的构造器。
> 
> 不能和 `super()` 一起使用

## 多态

在继承 / 实现情况下的一种现象，表现为对象多态、行为多态。

一般表现形式为父类类型指向一个子类的对象。

在多态下，只能访问父类的内容。如果子类对父类的方法重写，那么会访问到重写之后的方法。

### 抽象类

关键字 ` abstract `

**一、核心特性**

1. 抽象类（abstract class）
   - 不能直接实例化（必须通过子类继承）。
   - 可以包含 抽象方法（无实现）和 具体方法（有实现）。
   - 可以定义 成员变量、构造方法、静态方法。

2. 抽象方法（abstract method）
   - 只有声明，没有方法体（以分号 ; 结尾）。
   - 必须在子类中被重写（除非子类也是抽象类）。

3. 设计目的
   - 作为模板，强制子类实现特定行为（如 `Animal` 抽象类要求子类实现 eat()）。

{{<alert>}}
**注意事项**
{{</alert>}}
1. 构造方法的存在意义
   - 虽然抽象类不能实例化，但子类构造方法会隐式调用父类构造方法（通过 `super()` ）。
   - 可用于初始化抽象类的成员变量。
2. 与 `final` 冲突
   - `abstract` 和 `final` 不能共用：
     - `final` 类不能被继承 → 与 `abstract` 需要继承矛盾。
     - `final` 方法不能被重写 → 与 `abstract` 必须重写矛盾。

**关键字 `final`**

`final` 用于声明不可变的变量、方法或类，具体作用如下：

1. `final` 变量（常量）
   - 基本类型：值不可变（如 `final int x = 10;`）。
   - 引用类型：引用不可变（但对象内部状态可能可变）。

2. `final` 方法
   - 不能被子类重写（但可以重载）。
   - 适用于不希望子类修改的方法（如模板方法模式中的关键步骤）。

3. `final` 类
   - 不能被继承（如 `String`、`Integer` 等不可变类）。
   - 适用于安全性要求高的类（防止恶意子类化）。

{{<alert>}}
**注意事项**
{{</alert>}}

1. `final` 变量的初始化时机
   - 成员变量：必须在声明时、构造方法中或初始化块中赋值。
    ~~~java
    final int a;  
    public MyClass() { a = 10; } // 合法  
    ~~~
   - 局部变量：只需在使用前赋值一次。
    ~~~java
    final int b;  
    b = 20; // 合法 法      
    ~~~

2. `final` 与不可变对象
   
- `final` 只能保证引用不变，若对象本身可变（如 `List`），仍需通过其他方式（如 `Collections.unmodifiableList()`）实现真正不可变。
  
3. `final` 参数
   - 方法参数被 `final` 修饰后，不能在方法内重新赋值（但不会影响调用方传入的变量）。
    ~~~java
    void foo(final int x) {  
        x = 10; // 编译错误！  
    }  
    ~~~
4. `final` 与性能优化
   - JVM 可能对 `final` 变量进行内联优化（直接替换为常量值）。
   - 多线程环境下， `final` 变量能保证初始化安全（无需额外同步）。

5. `final` 与其他关键字的冲突
   - abstract： `final` 方法不能被重写，`abstract` 方法必须被重写 → 互斥。
   - volatile： `final` 变量本身线程安全，无需再加 `volatile`。

### 多态下的类型转换

必须是一个还原操作, 否则会出现 `ClassCastException` 类型转换异常

可以以使用 `instanceof` 方法进行检测，该方法会返回一个 boolean 值，表示是否为这个类型。
    

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

## 字符串

两种初始化方式

~~~java
String s1 = "Hello";  
String s2 = "Hello";  

String s3 = new String("Hello");  
String s4 = new String("Hello");  
~~~

**对比**
1. 直接赋值 `=`
   - 节省内存（复用常量池中的字符串）  
   - 性能更高（避免重复创建对象）  
    **底层机制**
    - 检查字符串常量池（String Pool），如果 `"Hello"` 已存在，则 `s2` 直接引用池中的对象。
    - `s1` 和 `s2` 指向同一个内存地址（`s1 == s2` 返回 `true` 。
2. new String()
   - 内存浪费（每次 new 都会创建新对象）  
   - 性能较低（无常量池优化）  
   **底层机制**
   - 强制在堆内存中创建新对象，即使 `"Hello"` 已在常量池中存在。
   - `s3` 和 `s4` 指向不同的内存地址（`s3 == s4` 返回 `false`）。

> 在 java 6 之前都是存储在方法区，并且是固定大小的。从 java 7 开始常量池移入堆内存，哈希表，可动态扩展。移入堆之后，允许被 GC 回收，避免永久代内存溢出。

一些**面试题**
> Q： `String s1 = new String("Hello");` 这段代码创建了几个对象？
> 
> A:  1 或 2 个：
> 1. 如果常量池没有 "Hello"，先创建字面量对象（入池），再 new 堆对象 → 共 2 个。
> 2. 如果常量池已有 "Hello"，只 new 堆对象 → 共 1 个。
>
> Q： `String s = "a" + "b"` 创建几个对象？
> 
> A：1 个（编译期优化为 "ab"，直接存入常量池）。
>
> Q：`String s = new String("a") + new String("b")` 创建几个对象？
> 
> A：堆中：2 个 "a" 和 "b" 的 String 对象 + 1 个 "ab" 的 StringBuilder 最终结果 → 至少 3 个，常量池："a" 和 "b" 的字面量对象（如果之前不存在）→ 最多 +2 个。

| 方法名                                        | 描述                                                      |
| --------------------------------------------- | --------------------------------------------------------- |
| int length();                                 | 获取字符串的长度                                          |
| char charAt(int index);                       | 根据索引获取对应的字符                                    |
| char[] toCharArray();                         | 将字符串转成字符数组                                      |
| boolean equals(Object obj)                    | 比较字符串的内容                                          |
| boolean equalsIgnoreCase(String str);         | 忽略大小写比较字符串内容                                  |
| String substring(int start);                  | 从start位置截取到末尾, 原串不变, 返回新串                 |
| String substring(int start,int end);          | 从start位置截取到end-1, 原串不变, 返回新串                |
| String replace(String oldStr, String newStr); | 可以将字符串中所有的oldStr替换成newStr, 原串不变返回新串  |
| boolean startsWith(String start);             | 判断字符串是否以start开头                                 |
| boolean endsWith(String end);                 | 判断字符串是否以end结尾                                   |
| boolean contains(String str);                 | 判断字符串是否包含另外一个字符串                          |
| int indexOf(String str);                      | 查询str在字符串第一次出现的位置, 返回是首字母出现的位置   |
| int lastIndexOf(String str)                   | 查询str在字符串最后一次出现的位置, 返回是首字母出现的位置 |
| String toUpperCase();                         | 将字符串的小写都变成大写, 原串不变,返回新串               |
| String toLowerCase();                         | 将字符串的大写都变成小写, 原串不变,返回新串               |
| String[] split(String str)                    | 按照切割点进行字符串切割, 将结果封装数组中返回            |

字符串的内存分配

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

<span id="lamdbaText"></span>

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
> 我自己的理解就是引用某个方法的方法体，而 Lambda 是定义抽象方法的方法体

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

## 异常处理

### Java 异常体系结构

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

### 自定义异常

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

### 异常处理

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


## 泛型
在 java 5 引入的特性，核心作用第一是类型安全，在编译的时候就能检查类型错误，第二是提高代码复用性，同一套逻辑可以适配多种数据类型，第三是避免一些强制类型转换，自动类型推断。  

泛型可以定义为任意的标识符，一般是大写字母。

### 泛型类

~~~java
public class 类名<泛型1,泛型2,....>{

}
~~~

在实例化对象时确定泛型的类型。

### 泛型接口

~~~java
public interface 接口名<泛型1,泛型2...>{

}
~~~

1. 定义接口实现类的时候直接确定, 将泛型接口转成普通类。
2. 定义接口的实现类的时候不确定, 你就需要将该泛型继续声明出去, 让使用实现类的人确定, 将泛型接口转成泛型类了。

### 泛型方法

~~~java
修饰符  <泛型1, 泛型2,....> 返回值类型  方法名(参数列表 ){
    方法体;
    return 返回值;
}
~~~

## 补充

System：数组copy
时间戳
