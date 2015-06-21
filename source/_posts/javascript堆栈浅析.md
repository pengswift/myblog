title: javascript堆栈浅析
date: 2015-06-15 10:59:30
tags: javascript
---
### 类型: 
	1.	基础类型   
	2.	引用类型
	
### 堆空间:
	1.  存储对象 e.g.  Object()
	
### 栈空间:
	1.  存储变量标示符  e.g.  var a;  var o = Object()  
		a -> 基础类型变量标示符
		o -> 对象引用变量标示符
		
	2.  存储变量的值    e.g.  var a = 100
		100 -> 基础类型变量的值
		
![](/img/堆栈分析1.png)

![](/img/堆栈分析2.png)

	