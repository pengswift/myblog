title: javascript-对象与原型
date: 2015-06-16 09:26:55
tags: javascript
---
### 理解对象:
对象是一种风格， 和其它语言的类的概念相似，类中主要定义了一些基本属性和方法。 
当类被通过构造使用时，实例对象也会拥有一份新的属性和方法（但此处方法不会被重复创建，还是指向同一个地址）

why？ 方法不需要被重复创建呢？
首先，我们理解一个概念，所有语言中最终回转化成汇编执行，即便是动态语言，底层也会有汇编入栈，出栈的这种操作。

当我们通过实例化的对象去调用的一个方法时，我们会取得当前方法的引用地址，并将参数一个个push进去，然后pop出结果即可， 所以说，完全不需要构造两个相同的方法，即使这样做了，也不会有什么影响，浪费内存空间而已。

但变量一般都是copy创建的，我们当然不希望多个实例修改同一个变量，最终得到的变量是一样的吧，如果真的有这样需求，在其它语言中该变量用static定义，而在javascript中，那得用到prototype原型了，说白了，就是只定义一处，大家都使用同一份的方法。

	1.工厂模式
	e.g.
	function createPerson(name, age, job) {
		var o = new Object();
		o.name = name;
		o.age = age;
		o.job = job;
		o.sayName = function() {
			alert(this.name);
		};
		return o;
	}
	
	var person1 = createPerson('Nicholas', 29, 'Software Enginner');
	var person2 = createPerson('Grey', 27, 'Doctor');
	
	问题:
	 	1. 每次创建方法都是一份新的地址 （重复）
	 	2. 没有知道该对象的具体类型，也就缺乏了内省机制，好比如说，你问它，你是谁？ 它说不知道。但如果它知道自身是什么，那么那就可以做很多事情了。 python和ruby就完全具备此特性
	 		
<br>

	2.构造函数模式
	e.g. 
	function Person(name, age, job) {
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function() {
			alert(this.name);
		};
	}
	
	var person1 = new Person('Nicholas', 29, 'Software Engineer');
	var person2 = new Person('Greg', 27, 'Doctor');
	
	问题:
		1. 每次创建方法都是一份新的地址 （重复）
		
	注: 此时使用构造函数方式创建对象时，对象内部会有一个constructor（构造属性）指向Person，表明时通过谁来创建的。 但constructor并不是实例的属性，它是指向Person原型的属性（即共用一份内存地址的属性）
	
<br>

	3.原型模式:
	e.g.
	function Person() {
	}
	
	Person.prototype.name = 'Nicholas';
	Person.prototype.age = 29;
	Person.prototype.job = 'Software Engineer';
	Person.prototype.sayName = function() {
		alert(this.name);
	};
	
	var person1 = new Person();
	person1.sayName();			//Nicholas
	
	var person2 = new Person();
	person2.sayName();			//Nicholas
	alert(person1.sayName == person2.sayName); // true
	
	好处:
		1.解决了多次创建同一份地址方法
	
	问题: 
		1.无法通过构造进行传参
		
		
	3.1 字面量方式-1:
	e.g.
	function Person() {
	}
	
	Person.prototype = {
		name: 'Nicholas',
		age: 29,
		job: 'Software Engineer',
		sayName: function() {
			alert(this.name);
		}
	}
	
	好处: 
		1.不用每次敲一遍Person.prototype
		
	问题:
		1.无法通过构造进行传参
		2.由于重写了默认的prototype, 即通过{}方式重新定义的对象， 因此，constructor属性也就变成了新的constructor属性，（指向了Object构造属性）,不在指向Person函数
		
	3.2 字面量方式-2:
	e.g.
	funciton Person() {
	}
	
	Person.prototype = {
		constructor: Person,
		name: 'Nicholas',
		age: 29,
		job: 'Software Engineer',
		sayName: function() {
			alert(this.name);
		}
	}
	
	好处: 
		1.不用每次敲一遍Person.prototype
		
	问题:
		1.无法通过构造进行传参
	
	
	3.3 字面量方式-3:
	e.g.
	function Person() {
	}
	
	Person.prototype = {
		constructor: Person,
		name: 'Nicholas',
		age: 29,
		job: 'Software Engineer',
		friends: ['Shelby', 'Court'],
		sayName: function() {
			alert(this.name);
		}
	}
	
	var person1 = new Person();
	var person2 = new person();
	
	person1.friend.push('Van');
	alert(person1.friends);   //'Shelby,Court,Van'
	alert(person2.friends);   //'Shelby,Court,Van'
	alert(person1.friends === person2.friends); //true
	
	
	好处: 
		1.不用每次敲一遍Person.prototype
		
	问题:
		1.无法通过构造进行传参
		2.由于在原型中使用了引用类型的属性，通过实例修改时，会影响到其它实例。但如果设计目的就是如何，便不再是问题

	
		
<br>
	
	4.组合使用构造函数模式和原型模式
	e.g.
	function Person(name, age, job) {
		this.name = name;
		this.age = age;
		this.job = job;
		this.friends = ['Shelby', 'Count'];
	}
	
	Person.prototype = {
		constructor: Person,
		sayName: function() {
			alert(this.name);
		}
	}
	
	var person1 = new Person('Nicholas', 29, 'Software Engineer');
	var person2 = new Person('Greg', 27, 'Doctor');
	
	person1.friends.push('Van');
	alert(person1.friends);   //'Shelby,Count,Van'
	alert(person2.friends);   //'Shelby,Count'
	alert(person1.friends === person2.friends); //false
	alert(person1.sayName === person2.sayName); //true
	
	优点：
		1.解决了构造函数传值的问题
		2.使用了简化版的字面量方式
		3.方法在内存中只存在一份
		4.引用类型不再共享一份。
		
	这种模式堪称完美

<br>
	
	5. 动态原型模式
	e.g.
	function Person(name, age, job) {
		this.name = name;
		this.age = age;
		this.job = job;
		
		if (typeof this.sayName != 'function') {
			Person.prototype.sayName = function() {
				alert(this.name);
			};
		}
	}
	
	优点: 
		1. 变量的定义和方法都存在一个函数内，和其它包含class风格的语言更加接近
		
	缺点:
		1. 使用这种方式时， 无法使用字面量方式重写原型方法，如果已经创建了实例的情况下重写原型，就会先切现有实例和新原型之间的联系。（所以说，这种方式不用自面量就好了）
		
	经典模式之一	
		
<br>

	6. 寄生构造函数模式
	e.g.
	function Person(name, age, job) {
		var o = new Object();
		o.name = name;
		o.age = age;
		o.job = job;
		o.sayName = function() {
			alert(this.name);
		};
		return o
	}
	
	var friend = new Person('Nicholas', 29, 'Software Engineer');
	friend.sayName(); //'Nicholas'
	
	注: 对于此模式，目前还未充分认识其价值，或许其中也蕴含宝藏，暂不评论
	
<br>

	7. 稳妥构造函数模式
	e.g.
	function Person(name, age, job) {
		var o = new Object();
		o.sayName = function() {
			alert(name);
		};
		return o;
	}
	
	var friend = Person('Nicholas', 29, 'Software Enginner');
	friend.sayName();
	
	注: 暂不评论
	
	
	
	
	
	
	