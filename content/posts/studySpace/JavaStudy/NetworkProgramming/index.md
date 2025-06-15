---
date: '2025-06-09T20:26:11+08:00'
draft: false
title: '网络编程'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 14  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---


## 协议

**UPD**：

面向无连接, 不可靠,以数据报的形式发送数据, 最大是64kb一帧数据, 速度快但是容易丢失数据。

**TCP**：

面向链接, 安全可靠, 发送之前会经过三次握手, 断开之前会经过四次挥手。

三次握手建立链接，保证信息通道正常
1. 客户端会发送SYN消息等待服务给确认。
2. 服务器发送ack确认消息, 然后并发送syn消息,然后等待客户端的确认。
3. 客户端发送ack确认消息

四次挥手，保证数据传输完毕。
1. 客户端发送FIN消息,等待服务器确认 fin_rev1。
2. 服务端,回复ack消息, 让客户端处于fin_rev2。
3. 服务器处理完数据后, 发送一个FIN消息等待客户端的确定自己处于rev_fin。
4. 客户端发送ack消息, 确认断开。

## UDP 通信

主要依赖两个类：

- `DatagramSocket` ：用于发送和接收 UDP 数据报。
- `DatagramPacket` ：表示要发送或接收的数据包。


**发送端基本流程**：

- 创建 `DatagramSocket` （可绑定端口，也可以让系统分配端口）。
- 构造要发送的数据，封装成 `DatagramPacket` 。
- 使用 `DatagramSocket.send(packet)` 发送数据。

**接收端基本流程**：

- 创建 DatagramSocket，绑定接收端口。
- 创建空的 DatagramPacket 用于接收数据。
- 使用 DatagramSocket.receive(packet) 接收数据。

### 代码示例：

发送端
~~~java
DatagramSocket ds = new DatagramSocket();

String msgStr ="hello word";

byte[] massage = msgStr.getBytes();

DatagramPacket dp = new DatagramPacket(massage, massage.length, InetAddress.getByName("127.0.0.1"),1589);

ds.send(dp);

ds.close();
~~~

接收端
~~~java
DatagramSocket ds = new DatagramSocket(1589);

DatagramPacket dp = new DatagramPacket(new byte[1024],1024);

System.out.println("开始接受数据");

ds.receive(dp);

System.out.println("接受成功");

byte[] msg = dp.getData();

int len = dp.getLength();

String msgStr = new String(msg,0,len);

System.out.println(msgStr);
~~~

## TCP 通信

主要依赖两个类：

- `ServerSocket` : 服务端专用，用于监听客户端连接
  ~~~java
    ServerSocket(int port)    // 创建绑定到指定端口的服务器套接字
    Socket accept()           // 监听并接受客户端连接(阻塞方法)
    void close()              // 关闭服务器套接字
  ~~~
- `Socket` : 客户端和服务端通信端点
  ~~~java
    Socket(String host, int port)  // 客户端构造方法
    InputStream getInputStream()   // 获取输入流
    OutputStream getOutputStream() // 获取输出流
    void close()                  // 关闭套接字
  ~~~

**创建TCP服务器基本流程**：

~~~java
//设置监听端口
ServerSocket server = new ServerSocket(8080);

//等待链接，会被阻塞
Socket client = server.accept();

//输入流对象
DataInputStream Indata = new DataInputStream(client.getInputStream());

//输出流对象
DataOutputStream outData = new DataOutputStream(client.getOutputStream());

//获取发送过来的数据，阻塞操作
String msg = Indata.readUTF();

//返回数据
outData.writeUTF(msg);
~~~

**创建 TCP 请求基本流程**：

~~~java
//设置请求服务器
Socket socket = new Socket("127.0.0.1", 8080);

//输出流对象
DataOutputStream dataOutput = new DataOutputStream(socket.getOutputStream());

//输入流对象
DataInputStream inData = new DataInputStream(socket.getInputStream());

//发送数据
dataOutput.writeUTF("nihao");

//读取发送过来的数据，阻塞操作
System.out.println(inData.readUTF());
~~~

## 粘包问题

如果使用 `socket.getInputStream()` 和 `socket.getOutputStream()` 直接读取或写入会有粘包问题，使用 `DataOutputStream` 和 `DataInputStream` 向包的尾部写了一个结束字符。保证不会发生粘包现象。

## 时间

## 反射

### 获取字节码

### 获取构造函数