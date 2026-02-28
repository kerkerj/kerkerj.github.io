---
date: 2016-10-25T12:32:55+08:00
slug: goroutine-discussion
draft: false
tags: ['golang', 'goroutine']
title: "goroutine 執行相關討論"
description: "從 Golang Taiwan Slack 上的討論出發，探討 goroutine 的執行順序、GOMAXPROCS 與 runtime.Gosched 的影響。"
---


幾天前在 Golang Taiwan 的 slack 裡看到了一些關於 goroutine 的討論

有人問了以下程式碼 (from http://blog.mergermarket.it/now-youre-thinking-with-channels/)

```c
package main

import "fmt"

func main() {
	channelForInts := make(chan int)
	go printIntFromChannel(channelForInts)
	channelForInts <- 5
}

func printIntFromChannel(channel chan int) {
	number := <-channel
	fmt.Println("The number is:", number)
}
```

提到上述程式碼在 goplayground 上測試好幾次都會印出 `The number is:5`

但**他自己在本機跑是怎樣執行都不會出現 5**

看到這個問題第一個想到的是 `sync.Waitgroup`

之前剛開始學 goroutine 的概念時，就在 stackoverflow 上看到有人討論相關的議題

加個 `sync.Waitgroup` 就可以保證看到 goroutine 執行完後的結果。

而事實上會這樣子的原因 [Go Programming Language Specification](https://golang.org/ref/spec#Program_execution) 有寫

> Program execution begins by initializing the main package and then invoking the function `main`. When that function invocation returns, the program exits. It does not wait for other (non-`main`) goroutines to complete.

所以在 main 執行完後也不管 goroutine 了，反正我就是要關掉了～這樣

改成用 `sync.Waitgroup` 的話大概就是長下面這樣，有稍微變形一下，多另外一個 goroutine 來往 channel 丟值

```c
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func main() {
	channelForInts := make(chan int)

	wg.Add(2)
	go setNum(channelForInts)
	go printIntFromChannel(channelForInts)

	wg.Wait()
}

func setNum(channel chan int) {
	defer wg.Done()
	
	for i := 0; i < 100; i++ {
		channel <- i
	}
	close(channel)
}

func printIntFromChannel(channel chan int) {
	defer wg.Done()

	for {
		number, ok := <-channel

		if !ok {
			break
		}

		fmt.Println("The number is:", number)
	}
}
```

另外社群朋友也提到另一個觀點，那就是將 `GOMAXPROCS` 設成 `1`

> The GOMAXPROCS variable limits the number of operating system threads that can execute user-level Go code simultaneously. There is no limit to the number of threads that can be blocked in system calls on behalf of Go code; those do not count against the GOMAXPROCS limit.

且 goroutine 內沒有 system call，就不會有 context switch

所以會等 goroutine 跑完後回 main 把整個 process 完成

也是挺有趣的，從沒想過這個地方。



最後就是看起來這兩個來源值得一讀

1. Golang 的 spec https://golang.org/ref/spec
2. Golang Memory Model https://golang.org/ref/mem