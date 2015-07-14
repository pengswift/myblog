title: golang-单向channel
date: 2015-07-07 23:18:42
tags: golang
---

e.g.

	package main
	
	func main() {
		c := make(chan int, 3)
		
		var send chan<- int = c
		var recv <-chan int = c
		
		send <- 1
		
		println(<-recv)
		
	}