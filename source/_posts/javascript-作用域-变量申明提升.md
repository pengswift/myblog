title: 'javascript-作用域&变量申明提升'
date: 2015-06-15 13:53:42
tags: javascript
---

###javascript 没有块级作用域，只有函数作用域
	var color = 'red';
	function A() {
		console.log(color);  //undefined
		var color = 'blue';
	}
	A();
	  
	equals:

	var color = 'red';
	function A() {
		var color;
		console.log(color);
		color = 'blue';
	}

	A();
	
	注: 变量color在函数题内被重新定义时，会进行申明提升， 提升到函数作用域顶部。