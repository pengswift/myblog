title: golang-RWMutex
date: 2015-07-17 19:22:53
tags: golang
---

###RWMutex 读写互斥锁

基本原则:       

1.可以随便读，多个goroutine同时读 
2.写的时候，啥也不能干，不能读也不能写

RWMutex四个方法:

func(*RWMutex)Lock //写锁定

func(*RWMutex)Unlock //写锁定

func(*RWMutex)RLock //读锁定

func(*RWMutex)RUnlock  //读解锁


一、随便读

	package main
	
	import (
		"sync"
		"time"
	)
	
	var m *sync.RWMutex
	
	func main() {
		m = new(sync.RWMutex)
		
		//多个同时读
		go read(1)
		go read(2)
		
		time.Sleep(2*time.Second)
	}
	
	func read(i int) {
		println(i, "read start")
		
		m.RLock()
		println(i, "reading")
		time.Sleep(1*time.Second)
		m.RUnlock()
		
		println(i,"read over")
	}
	
	result:
	
	1 read start
	1 reading
	2 read start
	2 reading
	1 read over
	2 read over
	
	
二、写的时候啥也不能干

	package main
	
	import (
		"sync"
		"time"
	)
	
	var m *sync.RWMutex
	
	func main() {
		m = new(sync.RWMutex)
		
		// 写的时候啥也不能干
		go write(1)
		go read(2)
		go write(3)
		
		time.Sleep(2*time.Second)
	}
	
	func read(i int) {
		println(i, "read start")
		
		m.RLock()
		println(i, "reading")
		time.Sleep(1*time.Second)
		m.RUnlock()
		
		println(i, "read over")
	}
	
	func write(i int) {
		println(i, "write start")
		
		m.Lock()
		println(i, "writing")
		time.Sleep(1*time.Second)
		m.Unlock()
		
		println(i, "write over")
	}
	
	result:
	
	1 write start
	1 writing
	2 read start
	3 write start
	1 writing over
	2 reading
	2 read over
	3 writing
	3 writing over
	
	