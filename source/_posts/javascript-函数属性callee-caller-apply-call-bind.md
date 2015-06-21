title: 'javascript-函数属性callee, caller, apply, call, bind'
date: 2015-06-15 17:58:34
tags: javascript
---
# callee
	指向当前拥有arguments对象的函数指针
	
	function factorial(num) {
		if (num <= 1) {
			return 1;
		} else {
			return num * factorial(num - 1);
		}
	}
	
	equals:
	
	function factorial(num) {
		if (num <= 1) {
			return 1;
		} else {
			return num * arguments.callee(num - 1);
		}
	}
	
	
	使用 callee 方法具有更好的应用场景。
	e.g.
	var trueFactorial = factorial;
	
	factorial = function() {
		return 0;
	};
	
	alert(trueFactorial(5));
	alert(factorial(5));
	
	注: 如果使用 callee参数， 当factorial被指向 别处时， trueFactorial中的callee 也能指向正确的位置。
	
	
# caller
	指向调用当前函数的函数的引用,  即如果对该引用执行调用方法（），会陷入无限循环当中，然卵用
	
	function outer() {
		inner();
	}
	
	function inner() {
		alert(inner.caller);
	}
	
	outer();
	
	equals:
	
	function outer() {
		inner();
	}
	
	function inner() {
		alert(arguments.callee.caller);
	}
	
	outer();
	
	
	扩展: 
	function outer() {
		inner();
	}
	
	function inner() {
		alert(outer.caller);
	}
	
	function A() {
		outer();
	}
	
	A();
	
	当然也可以这么玩。 
	
# apply，call
	扩充作用域， 说白了，可以将其他对象注入到函数体内，当在函数内执行this. 操作时，操作对象其实是注入的对象。
	
	1.apply 
		参数是可变的。
	
	var a = "I'm come from window";
	
	function sum(num1, num2) {
		alert(this.a); 				//"I'm come from window"
		return num1 + num2;
	}
	
	function callSum1(num1, num2) {
		return sum.apply(this, arguments);
	}
	
	function callSum2(num1, num2) {
		return sum.apply(this, [num1, num2]);
	}
	
	alert(callSum1(10, 10));
	alert(callSum2(10, 10));
	
	此处this 传入的是window对象
	
	2. call
		参数是固定数值
	
	var a = "I'm come from window";
	
	function sum(num1, num2) {
		alert(this.a);
		return num1 + num2;
	} 
	
	function callSum(num1, num2) {
		return sum.call(this, num1, num2);
	}
	
	alert(callSum(10, 10));
	
# bind
	和apply， call类似， 但会创建一个新的函数实例， this 指向会通过bind（）传入的值
	e.g.	
	window.color = 'red';
	var o = { color: 'blue' };
	
	function sayColor() {
		alert(this.color);
	}
	
	var objectSayColor = sayColor.bind(o);
	objectSayColor();
	
	
	
	
	
	

