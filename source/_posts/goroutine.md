---
title: Go并发编程示例
author: Sofar
date: 2021-06-01 18:32:58
tags:
- 并发编程 
categories: go
---

Go 语言提供了 sync 和 channel 两种方式支持协程(goroutine)的并发。

例如我们希望并发下载 N 个资源，多个并发协程之间不需要通信，那么就可以使用 sync.WaitGroup，等待所有并发协程执行结束。

## 1.sync


```go

import (
	"fmt"
	"sync"
	"time"
)

var wg sync.WaitGroup

func download(url string) {
	fmt.Println("start to download", url)
	time.Sleep(time.Second) // 模拟耗时操作
	wg.Done()
}

func main() {
    wg.Add(3)
	for i := 0; i < 3; i++ {
		go download("a.com/" + string(i+'0'))
	}
	wg.Wait()
	fmt.Println("Done!")
}

```


+ wg.Add(3) : 为wg添加一个计数
+ wg.Done() : 每执行wg.Done()一次，减去一个计数
+ wg.Wait() : 阻塞等待wg计数等于0
+ go download() : go 开启新的协程的关键字， 在新的协程中执行download()函数


## 2.channel
使用 channel 信道，可以在协程之间传递消息。阻塞等待并发协程返回消息。

```go

var ch = make(chan string, 10) // 创建大小为 10 的缓冲信道

func download(url string) {
	fmt.Println("start to download", url)
	time.Sleep(time.Second)
	ch <- url // 将 url 发送给信道
}

func main() {
	for i := 0; i < 3; i++ {
		go download("a.com/" + string(i+'0'))
	}
	for i := 0; i < 3; i++ {
		msg := <-ch // 等待信道返回消息。
		fmt.Println("finish", msg)
	}
	fmt.Println("Done!")
}

```

## 3.读写锁
并发时注意对资源的占用。
   
小技巧：
在读锁占用的情况下，会阻止写但不阻止读，写锁会阻止读写，所以add的时候开启读锁而不是写锁可以提高性能.

```go

var counter int = 0
func main() {
	start := time.Now()
	lock := &sync.RWMutex{}
	for i := 0; i < 10; i++ {
		go add(1, i, lock)
	}

	for  {
		lock.RLock()
		c := counter
		lock.RUnlock()
		runtime.Gosched()
		if c >= 10 {
			break
		}
	}
	end := time.Now()
	consume := end.Sub(start).Seconds()
	fmt.Println("程序执行耗时(s)：", consume)
}

func add(a, b int, lock *sync.RWMutex)  {
	c := a + b
	lock.Lock()
	counter++
	fmt.Printf("%d: %d + %d = %d\n", counter, a, b, c)
	lock.Unlock()
}

```

