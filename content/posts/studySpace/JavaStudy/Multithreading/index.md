---
date: '2025-06-09T20:31:23+08:00'
draft: false
title: '多线程'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列
series_order: 13  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

进程：操作系统分配资源的最小单位。

线程：操作系统调度的最小单位。

**并发执行任务的原理：**  
当我们调用线程的start() 方法时, 会执行本地方法start0(), 这个本地方法会开辟一个新的线程, 并且为其分配独立的栈空间。
而且自动执行run方法在这个栈空间加载执行, 而我们cpu可以在多个栈空间的栈顶, 并发执行任务!!!

## 线程的状态

1. `NEW` ：新建状态，线程对象已创建但未启动。
2. `RUNNABLE` ：可运行状态，线程正在运行或准备运行。
3. `BLOCKED` ：阻塞状态，等待锁资源。
4. `WAITING/TIMED_WAITING` ：等待状态，调用 `wait()/sleep()` 等。
5. `TERMINATED` ：终止状态，线程运行完或异常退出。


## 创建线程的方法

### 继承 Thread 类
~~~java
class MyThread extends Thread {
    public void run() {
        System.out.println("线程执行中：" + Thread.currentThread().getName());
    }
}

//启动线程
MyThread t1 = new MyThread();
t1.start();
~~~

> **缺点**: 单一继承的局限性, 任务和线程绑定紧密, 耦合度太高了
> 
> **优点**: 可以直接使用 Thread 的功能, 比如直接使用 start 方法开启线程

### 实现 Runnable 接口（推荐方式）

~~~java 
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("线程执行中：" + Thread.currentThread().getName());
    }
}

//启动线程
Thread t2 = new Thread(new MyRunnable());
t2.start();
~~~

> **优点**: 线程和任务是分离,解耦合的, 解决单一继承的局限
> 
> **缺点**: 不能直接使用thread的功能, 但是后期可以通过 Thread 提供的方法解决该问题 Thread.currentThread();

### 使用匿名类创建线程

~~~java
// 第一种:
new Thread(){
    run(){
    }
}.start();

// 第二种:
new Thread(new Runnable(){
    run(){

    }
}).start();
~~~

## 常用的线程方法

| 方法名                        | 作用                                             |
| ----------------------------- | ------------------------------------------------ |
| `run()`                       | 存放任务的地方, 我们也可以手动调用run!!!         |
| `start()`                       | 开启线程                                         |
| `String getName()`              | 获取线程的名字                                   |
| `void setName(String name) `    | 设置线程的名字                                   |
| `static Thread currentThread()` | 获取正在执行代码的当前线程对象                   |
| `static void sleep(long time)`  | 让当前线程处于休眠状态                           |
| `void join()`                   | 插队, 插队其实是将两个线程栈的任务合并一个线程中 |

## 线程安全 & 同步

**同步代码块**
~~~java
synchronized(锁对象){
    需要同步的代码
}
~~~

锁对象: 可以是任意对象,但是必须保证锁对象的唯一

**同步方法**

~~~java
public synchronized void method() {
    // 同步代码
}
~~~

{{<alert>}}
一旦锁,锁住是整个方法。  
不能锁住不同空间的代码,因为锁对象不能自由设置是写死的!!!!
{{</alert>}}

**使用 ReentrantLock（功能更强）**

~~~java
Lock lock = new ReentrantLock();
lock.lock();
try {
    // 线程安全代码
} finally {
    lock.unlock();
}
~~~

## 线程池

Java线程池框架的核心是 `Executor` 框架。

### 线程池创建

1. **通过 Executors 工厂类创建**

~~~java
// 固定大小的线程池
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(5);

// 可缓存的线程池（线程数自动调整）
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();

// 单线程的线程池（保证任务顺序执行）
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();

// 支持定时及周期性任务执行的线程池
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(3);
~~~

2. 直接创建 ThreadPoolExecutor 

~~~java
ExecutorService customThreadPool = new ThreadPoolExecutor(
    5, // 核心线程数
    10, // 最大线程数
    60L, // 空闲线程存活时间
    TimeUnit.SECONDS, // 时间单位
    new ArrayBlockingQueue<>(100) // 工作队列
    ThreadFactory threadFactory,  //创建线程的工程
    RejectedExecutionHandler handler // 拒绝策略
);
~~~

3. 提交任务
~~~java
submit();
execute();
~~~

### 任务队列类型

- `ArrayBlockingQueue` ：基于数组的有界队列。
- `LinkedBlockingQueue` ：基于链表的无界队列（默认Integer.MAX_VALUE）。
- `SynchronousQueue` ：不存储元素的队列，每个插入操作必须等待一个移除操作。
- `PriorityBlockingQueue` ：具有优先级的无界队列。

### 拒绝策略

- `AbortPolicy` （默认）：直接拒绝并抛出异常。（RejectedExecutionException）
- `DiscardPolicy` ：直接拒绝不抛出异常。
- `DiscardOldestPolicy` ：直接拒绝等待最久的那个任务。
- `CallerRunsPolicy` ：用调用者所在线程来运行任务。

### 线程池工作流程

1. 提交任务后，如果当前线程数 < corePoolSize，创建新线程执行任务。
2. 如果线程数 >= corePoolSize，将任务放入工作队列。
3. 如果队列已满且线程数 < maximumPoolSize，创建新线程执行任务。
4. 如果队列已满且线程数 >= maximumPoolSize，执行拒绝策略。

### 线程池监控方法

~~~java
// 获取当前线程数
int poolSize = executor.getPoolSize();

// 获取活跃线程数
int activeCount = executor.getActiveCount();

// 获取已完成任务数
long completedTaskCount = executor.getCompletedTaskCount();

// 获取任务队列中的任务数
int queueSize = executor.getQueue().size();
~~~
