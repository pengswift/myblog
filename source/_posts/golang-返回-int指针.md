title: 'golang-返回*int指针'
date: 2015-07-07 16:46:36
tags: golang
---

func test() *int {
	x := 100
	return &x
}

注: 此处因为返回了变量int的地址， 所以基本类型x的值不会分配到栈中， 
	因为栈中的数据只在作用域范围内有效。
	所以此处编译器会根据需求将其分配到堆中，交给gc去处理