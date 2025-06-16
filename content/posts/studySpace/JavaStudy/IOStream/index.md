---
date: '2025-06-15T22:21:49+08:00'
draft: false
title: 'I/O流、文件处理'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 10  #系列编号
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

## 文件处理

创建文件对象
~~~java
File file = new File(path);
~~~

常用方法
| 方法名                   | 描述                                          |
| ------------------------ | --------------------------------------------- |
| `file.exists()`          | 路径是否存在。                                |
| `file.isFile()`          | 是否为文件。                                  |
| `file.isDirectory()`     | 是否为文件夹，如果路径不存在，则返回 false 。 |
| `file.getPath()`         | 获取创建文件对象时的路径。                    |
| `file.getAbsolutePath()` | 获取绝对路径。                                |
| `file.lastModified()`    | 获取上一次修改的毫秒值。                      |
| `file.length()`          | 获取文件或者文件夹的大小。                    |
| `file.getName()`         | 获取的文件或者文件夹的名字。                  |
| `file.mkdir()`           | 创建文件夹，如果目录不存在，则创建失败。      |
| `file.mkdirs()`          | 创建目录。                                    |
| `file.delete()`          | 只能删除文件或空文件夹。                      |
| `file.listFiles()`       | 获取当前文件夹下的文件集合，返回 `File[]`     |







IO流是Java中用于输入和输出操作的API，主要位于java.io包中。它提供了处理不同类型数据流（如字节流、字符流等）的类和接口。

## 字节流

**InputStream(抽象类) 输入流**

必须保证文件存在

- *创建文件输入流*  
  `FileInputStream(File file)`  以读取由指定的 File对象表示的文件。  
  `FileInputStream(String name)`  创建文件输入流以指定的名称读取文件。  

- *常用方法*   
  `int read()`  一次读一个，返回得到的字节，-1停止从输入流中读取下一个数据字节。  
  `int read(byte[] b)` ，一次读多个, 将读到的内容放到数组中, 返回读取的有效个数-1停止。  
  `void close();` 关闭资源

**OutputStream(抽象类) 输出流**
- FileOutputStream  
  `FileOutputStream(File file)`  创建文件输出流以写入由指定的 File对象表示的文件。  
  `FileOutputStream(File file, boolean append)` 续写，创建文件输出流以写入由指定的 File对象表示的文件。  
`FileOutputStream(String name)` 创建文件输出流以指定的名称写入文件。  
`FileOutputStream(String name, boolean append)` 续写,创建文件输出流以指定的名称写入文件。  

## 字符流

**码表**
- `ASCII` ：单个字节，里面只有英文大小写字母, 数字, 英文符号等。
- `GBK` ：两个字节，常见的汉字以及ASCII码字符。
- `unicode`：  
  `utf-8`：中文在utf-8中一般占用3个字节，4个字节。

### 编码和解码
    编码:  字符 ---> 字节
    解码:  字节 ---> 字符

**编码**  
| 方法名                                 | 描述             |
| -------------------------------------- | ---------------- |
| `byte[] getBytes();`                   | 采用平台默认码表 |
| `byte[] getBytes(String charsetName);` | 采用指定码表编码 |

**解码**  
| 方法名                                                          | 描述                                 |
| --------------------------------------------------------------- | ------------------------------------ |
| `String(byte[] arr); `                                          | 按照平台默认码表转成字符串           |
| `String(byte[] arr, int start, int length);`                    | 按照平台默认码表将指定内容转成字符串 |
| `String(byte[] arr,String charsetName);`                        | 按照指定码表转成字符串               |
| `String(byte[] arr, int start, int length,String charsetName);` | 按照指定码表将指定内容转成字符串     |



**Reader(字符输入流) 抽象类**

- *创建 FileReader*：  
  `FileReader(String name);`  
  `FileReader(File file);`  
- *方法*:  
  `int read();` 读单个字符, 返回读取的字符。  
  `int read(char[] arr);` 读多个字符到数组中, 返回读取有效个数。  
  `void close();` 关闭资源。  

**Writer(字符输出流)抽象类**

- *创建 FileWriter*  
    `FileWriter(File file)`  
    `FileWriter(File file, boolean append)`  
    `FileWriter(String fileName)`  
    `FileWriter(String fileName, boolean append)`  
- *方法*  
    `write(int ch);` 写单个字符  
    `write(char[] cbuf);` 写字符数组  
    `write(char[] arr, int start, int length);`  
    `write(String str);` 只会用这个!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  
    `write(String str, int start, int length);`  
    `flush();`  写入文件
    `close();`  

字符流内部为了一个字符数组，不能直接写入文件，需要调用 `flush()` 方法写入磁盘


## 缓冲流


### 缓冲字节流
> 不咋用，底层还是调用的字符流。

`BufferedInputStream(InputStream in);`  字节流的方法他都有

`BufferedOutputStream(OutputStream out);`  字节流的方法他都有


### 缓冲字符流

**读取**
`BufferedReader(Reader reader);` 方法和FileReader完全一样

特有方法:  
`String readLine();` 一次读一样,不读换行符 , 返回该行数据

**写入**
`BufferedWriter(Writer write);` 方法和FileWriter完全一样

特有方法:  
`void newLine();` 根据操作系统写出换行符

> 不同操作系统的换行符  
> window: \r\n  
> linux: \n  
> mac: \r  

{{<alert>}}
配合 1.7 之后新的 try cache 会让代码看起来更加简单
{{</alert>}}

