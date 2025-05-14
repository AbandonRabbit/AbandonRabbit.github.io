---
date: '2024-08-09T15:33:12+08:00'
draft: false
title: '并发提升思路'
seriesOpened: false #s是否开启系列
# series: [""] #属于的系列 
# series_order: 0  #系列编号
showSummary: ["go语言实现hook效果"] #摘要信息
tags: ["Golang基础"]
Categories: ["Golang","菜鸟提升"]
layoutBackgroundBlur: false #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---


## 如有以下程序：

~~~go
package main

import (
	"fmt"
	"time"
)

func main() {
	input := []int{1, 2, 3, 4, 5}
	output := []int{}

	start := time.Now()
	for _, i := range input {
		result := processData(i)
		output = append(output, result)
	}
	fmt.Printf("time cost : %v\n", time.Since(start))
	fmt.Println(output)

}

func processData(val int) int {
	time.Sleep(1 * time.Second)
	return val * 2
}
~~~

以上程序耗时大约 `5.0487571s`

## 尝试使用携程并加锁

~~~go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	input := []int{1, 2, 3, 4, 5}
	output := []int{}
	var wg sync.WaitGroup
	var mutex sync.Mutex
	start := time.Now()
	for _, i := range input {
		wg.Add(1)
		go func(val int) {
			defer wg.Done()
			mutex.Lock()
			result := processData(val)
			output = append(output, result)
			mutex.Unlock()
		}(i)
	}
	wg.Wait()
	fmt.Printf("time cost : %v\n", time.Since(start))
	fmt.Println(output)

}

func processData(val int) int {
	time.Sleep(1 * time.Second)
	return val * 2
}

~~~

执行耗时 ` 5.0556264s`

可以看到比不适用携程反而更慢了

# 第一种优化思路

观察程序发现 `result := processData(val)` 不算临界区资源，故可将协程代码修改如下

~~~go
go func(val int) {
    defer wg.Done()
    result := processData(val)
    mutex.Lock()
    output = append(output, result)
    mutex.Unlock()
}(i)
~~~

运行耗时 ` 1.0095853s`

## 思路总结

只对共享资源加锁，如果用来保护其他的像一些比较耗时的操作就体现不了并发性，会变成一个瓶颈

# 第二种优化思路

不使用共享资源

~~~go
func main() {
	input := []int{1, 2, 3, 4, 5}
	output := make([]int, len(input)) //[0,0,0,0,0]
	var wg sync.WaitGroup
	start := time.Now()
	for i, v := range input {
		wg.Add(1)
		go func(val int, p *int) {
			defer wg.Done()
			result := processData(val)
			*p = result
		}(v, &i)
	}
	wg.Wait()
	fmt.Printf("time cost : %v\n", time.Since(start))
	fmt.Println(output)

}
~~~

运行耗时 `1.0063716s`

## 思路总结

将共享资源进行分割，或者说给每个协程指定资源，*使用指针确定每个协程访问的资源是那个* ，

# 总结

在使用协程的时候，要注意临界资源和共享资源的使用，只对共享资源加锁，或者对共享资源进行划分，给每个进程指定可以访问的资源