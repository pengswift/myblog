title: golang-sync WaitGroup
date: 2015-06-27 22:23:55
tags: golang
---
##WaitGroup

用途: 它能够等到所有goroutine执行完成,并 <font color="red">**阻塞**</font> 主线程的执行，直到所有goroutine执行完成


在一个任务 依赖 其它多个任务的执行结果时， 并且其它任务是各自互不依赖，各自独立，就能最大发挥并行的优势， 此时主线程是阻塞的

WaitGroup方法

	1.Add: 添加或者减少等待goroutine的数量
	
	2.Done: 同 Add(-1)
	
	3.Wait: 执行阻塞， 直到所有WaitGroup数量变成 0 
	
	e.g.
	
	package main
    import (
       "fmt"
       "sync"
       "time"
    )
    
    func main() {
    	var wg sync.WaitGroup
    	for i := 0; i < 5; i++ {
    		wg.Add(1)
    		go func(n int) {
    			defer wg.Add(-1)
    			EchoNumber(n)
    		}(i)
    	}
    	wg.Wait()
    }
    
    func EchoNumber(i int) {
    	time.Sleep(3e9)
    	fmt.Println(i)
    }