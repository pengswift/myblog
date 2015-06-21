title: javascript-继承
date: 2015-06-16 17:41:34
tags: javascript
---
### 理解继承:

继承用于在创建子类时，可以调用父类的方法和使用父类定义的变量。 javascript本身没有提供继承的关键字， 而是通过原型链的方式，达到的继承这种效果（即调用父类方法，父类基本变量copy）， 然而javascript 对于父类引用类型的，子类在使用时，使用的是同一份地址，所以如果需求是如此，就没问题， 否则，需要重新在子类中重新定义自己的引用类型，因为javascript没有static 这个含义。

下面会通过一系列演变，最终达到我们所希望的继承效果。

	1. 原型链
	e.g.
	function SuperType() {
		this.property = true;
		this.colors = ['red','blue','green'];
	}
	
	SuperType.prototype.getSuperValue = function() {
		return this.property;
	};	
	
	function SubType() {
		this.subproperty = false;
	}
	
	SubType.prototype = new SuperType();
	
	SubType.prototype.getSubValue = function() {
		return this.subproperty;
	};
	
	var instance1 = new SubType();
	alert(instance1.getSuperValue()); 
	
	instance1.colors.push('black');
	alert(instance1.colors);
	
	var instance2 = new SubType();
	alert(instance2.colors);
	
	问题: 
		1. 父类定义的引用类型变量，子类在创建时会拥有改变量，但此时效果如何在子类上创建一个原型引用属性一样， 而通过子类创建的变量在修改时，会修改到同一份地址，这必然不是我们想要的
		2. 在创建子类同时，不能像父类构造函数传递参数	
<br>

	2.借用构造函数
	e.g.
	function SuperType(name) {
		this.name = name;
		this.colors = ['red', 'blue', 'green'];
		this.sayColor = function() {
		return this.colors;
	}

	function SubType() {
		SuperType.call(this,' Nicholas');
		this.age = 29;
	}

	var instance1 = new SubType();
	instance1.colors.push('black');
	alert(instance1.colors);

	var instance2 = new SubType();
	alert(instance2.colors);

	alert(instance1.name);
	alert(instance1.age);
	alert(instance1.sayColor == instance2.sayColor); //false

	
	优点:
		1.通过 call 改变父类作用域，达到了对于引用类型不共用同一份的问题。
	
	问题:
		1.但无法使用函数复用了，在实例子类时，父类定义的函数也会重新copy

<br>

	3.组合继承
	e.g.
	function SuperType(name) {
		this.name = name;
		this.colors = ['red', 'blue', 'green'];
	}

	SuperType.prototype.sayName = function() {
		alert(this.name);
	};

	function SubType(name, age) {
		SuperType.call(this,name);
		this.age = age;
	}

	SubType.prototype = new SuperType();
	SubType.prototype.constructor = SubType();
	SubType.prototype.sayAge = function() {
		alert(this.age);
	};

	var instance1 = new SubType('Nicholas', 29);
	instance1.colors.push('black');
	alert(instance1.colors);
	instance1.sayName();
	instance1.sayAge();

	var instance2 = new SubType('Greg', 27);
	alert(instance2.colors);
	instance2.sayName();
	instance2.sayAge();	
	
	优点:
		1.融合原型链和借用构造函数， 堪称完美的方式
	
	问题: 
		1.美中不足是 SuperType() 会调用2次
		
<br>

	4.原型式继承
	e.g.
	function object(o) {
		function F() {}
		F.prototype = o;
		return new F();
	}

	var person = {
		name:'Nicholas',
		friends: ['Shelby', 'Court', 'Van'],
		sayColor: function() {
			return 'blue';
		}
	};

	var anotherPerson = object(person);
	anotherPerson.name = 'Greg';
	anotherPerson.friends.push('Rob');

	var yetAnotherPerson = object(person);
	yetAnotherPerson.name = 'Linda';
	yetAnotherPerson.friends.push('Barbie');
	alert(person.friends);
	alert(anotherPerson.sayColor == yetAnotherPerson.sayColor);

	equals: 
	ECMAScript5版本
	
	var person = {
		name:'Nicholas',
		friends: ['Shelby', 'Court', 'Van'],
		sayColor: function() {
			return 'blue';
		}
	};

	var anotherPerson = Object.create(person);
	anotherPerson.name = 'Greg';
	alert(anotherPerson.name);
	anotherPerson.friends.push('Rob');

	var yetAnotherPerson = Object.create(person);
	alert(yetAnotherPerson.name);
	yetAnotherPerson.name = 'Linda';
	alert(yetAnotherPerson.name);

	yetAnotherPerson.friends.push('Barbie');
	alert(person.friends);
	alert(anotherPerson.sayColor == yetAnotherPerson.sayColor);

	
	注: 原型式继承也是一种不错的继承方式, 子类变量的修改并不会影响父类的prototype属性，方法也达到了共用, 唯一有点影响的事, 引用类型会始终共享相应的值。
	
<br>

	5. 寄生式继承
	e.g.		
	function createAnother(original) {
		var clone = Object.create(original);
		clone.sayHi = function() {
			alert("hi");
		};
		return clone;
	}

	var person = {
		name: 'Nichoals',
		friends: ['Shelby', 'Court', 'Van']
	};

	var anotherPerson = createAnother(person);
	anotherPerson.sayHi();
	
	注: 使用集成式函数，缺点是函数不能复用, 这一点同构造函数类似
	
<br>

	6.寄生组合式继承
	e.g.
	function inheritPrototype(subType, superType) {
		var prototype = Object.create(superType.prototype);
		prototype.constructor = subType;
		subType.prototype = prototype;
	}
	
	function SuperType(name) {
		this.name = name;
		this.colors = ['red', 'blue',' green'];
	}

	SuperType.prototype.sayName = function() {
		alert(this.name);
	};

	function SubType(name, age) {
		SuperType.call(this, name);
		this.age = age;
	}

	inheritPrototype(SubType, SuperType);

	SubType.prototype.sayAge = function() {
		alert(this.age);
	};

	var instance1 = new SubType('Nicholas', 29);
	instance1.colors.push('black');
	alert(instance1.colors);
	instance1.sayName();
	instance1.sayAge();

	var instance2 = new SubType('Greg', 27);
	alert(instance2.colors);
	instance2.sayName();
	instance2.sayAge();
	
	注: 完美
	
	
	
