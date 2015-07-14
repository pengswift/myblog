title: golang-Benchmark
date: 2015-07-07 18:15:48
tags: golang
---

e.g.
	
	package my
	
	import(
		"sync"
		"testing"
	)
	
	var lock sync.Mutex
	
	func test() {
		lock.Lock()
		lock.Unlock()
	}
	
	func testdefer() {
		lock.Lock()
		defer lock.Unlock()
	}
	
	func BenchmarkTest(b *testing.B) {
		for i := 0; i < b.N; i++ {
			test()
		}
	}
	
	func BenchmarkTestDefer(b *testing.B) {
		for i := 0; i < b.N; i++ {
			testdefer()
		}
	}

命令: 	
go test defer_b_test.go -bench=".*"