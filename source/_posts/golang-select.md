title: golang-select
date: 2015-06-29 12:41:02
tags: golang
---

##Select:

用途: 监听IO操作，当IO操作发生时，触发相应的动作, 与goroutine联合使用

	e.g.
	
	package main
	import(
		"fmt"
		"time"
	)
	
	func main() {
		c := make(chan int)
		
		go func() {
			time.Sleep(5 * time.Second)
			c <- 10
		}()
		
		for {
			select {
				case <-c:
				fmt.Print("trigger from chan C")
				return
			}
		}
	}


