title: javascript-值传递
date: 2015-06-15 12:14:55
tags: javascript
---

###公理: 
	1. 基础类型变量的直接赋值是值传递
	e.g.
	var num1 = 5;
	var num2 = num1;
	num1 = 10;
	alert(num2);   //5
	
######	
	
	2. 引用类型变量的直接赋值是引用传递
	e.g.
	var obj1 = new Oject();
	var obj2 = obj1;
	obj1.name = 'Nicholas';
	alert(obj2.name);     //"Nicholas"

######		
	
	3. javascript 中所有函数的参数都是按值传递的。
	e.g.
	
	基础类型:
	function addTem(num) {
		num += 10;
		return num;
	}
	
	var count = 20;
	var result = addTem(count);
	alert(count);		//20
	alert(result);		//30
	
	
	对象类型
	function setName(obj) {
		obj.name = 'Nicholas';
	}
	
	var person = new Object();
	setName(person);
	alert(person.name);   //'Nicholas'
	
	分析: 
		setName(person) 此处时值传递。
		obj对象是person的指针地址的copy（栈中的指针地址copy）。 所以说两者还是指向同一份堆地址， 即同一份对象
	
		当obj.name 改变时，改变的是同一份对象。
		
		所以说此处的值传递 ＝ 对象引用地址的copy !
	
	在证
	function setName(obj) {
		obj.name = 'Nicholas';
		obj = new Object();
		obj.name = 'Greg';
	}
	
	var person = new Object();
	setName(person);
	alert(person.name);      //"Nicholas"
	
	分析：
		当执行obj = new Object()；时， obj会指向堆空间中一份新的object对象
		
		当执行obj.name = 'Grey'；时，实际操作的是一个新的对象
		
		而 person 还是指向原先在堆中的地址， 
		在setName 方法内，obj.name 将堆中的数值进行了改变，
		此后obj又指向一个新的对象，再次改变其属性时，对原先对象没有影响。
		
		

###注意: 
	1.当复制保存对象的某个变量时，操作的是对象的引用。
	2.当在为对象添加属性时，操作的是实际的对象
	
	
	


	