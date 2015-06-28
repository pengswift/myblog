title: golang-sync.Mutex
date: 2015-06-28 23:13:22
tags: golang
---
Mutex
用途: 排它锁, 有锁定和未锁定状态， 当处于锁定状态时， 其它尝试进行的锁定操作都被等待，直到解锁

	package main
	import (
    	"fmt"
    	"sync"
    	"time"
	)

	var m *sync.Mutex

	func main() {
 		m = new(sync.Mutex)
    	go lock(1)
   	    time.Sleep(time.Second)
    	lock(2)
    	fmt.Printf("%s\n", "exit!")
	}

	func lock(i int) {
    	println(i, "lock start")
        m.Lock()
    	println(i, "lock")
    	time.Sleep(10 * time.Second)
    	m.Unlock()
    	println(i, "unlock")
	}
