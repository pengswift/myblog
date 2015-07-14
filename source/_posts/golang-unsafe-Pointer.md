title: golang-unsafe.Pointer
date: 2015-07-07 14:50:46
tags: golang
---

###unsafe.Pointer 
含义: 类似C的void *, 在golang中用于各种指针转换的桥梁


###unsafe.Pointer 与 uintptr
相同点: 两者都可以用于转换不同类型指针

区别: uintptr 可用于指针运算，unsafe.Pointer 则不可以


e.g.
	
	package main
	
	import(
		"fmt"
		"unsafe"
	) 
	
	type V struct {
		i int32
		j int64
	}
	
	func (this V) PutI() {
		fmt.Printf("i=%d\n", this.i)
	}
	
	func (this V) PutJ() {
		fmt.Printf("j=%d\n", this.j)
	}
	
	func main() {
		var v *V = new(V)
		var i *int32 = (*int32)(unsafe.Pointer(v))
		*i = int32(98)
		var j *int64 = (*int64)(unsafe.Pointer(uintptr(unsafe.Pointer(v)) + uint    ptr(unsafe.Sizeof(int32(1))))) 	  
		*j = int64(123)
		v.PutI()
		v.PutJ() 	
	}
	
	输出: 
	i=98
	j=0
	
<font color="red">注: </font>此处结果并不是我们想得到的结果, j值我们理想的值是123，但实际输出是0， 这是由于<font color="red">内存对齐规则</font>造成的

内存对齐规则:
参考: http://www.cppblog.com/snailcong/archive/2009/03/16/76705.html


因为内存对齐的影响， 结构体V中的i的占用8个字节，64位， 所以，要修改j的值， 指针要在i指针的基础上＋8个字节的长度， 也就是 unsafe.Sizeof(int64(1))

即:
 	var j *int64 = (*int64)(unsafe.Pointer(uintptr(unsafe.Pointer(v)) + uint    ptr(unsafe.Sizeof(int64(1)))))  
	
	
	