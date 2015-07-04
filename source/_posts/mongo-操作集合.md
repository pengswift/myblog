title: mongo-操作集合
date: 2015-06-30 11:18:38
tags: mongodb
---

1.插入命令

	1.1 单个插入
	db.foo.insert({"bar": "baz"})
	
	1.2 批量插入
	db.foo.insert([{"_id": 0}, {"_id" : 1}, {"_id" : 2}])
	
2.删除命令

	2.1 条件删除
	db.foo.remove({"_id": 0})
	
	2.2 全部删除
	db.foo.remove({})
	db.foo.drop()		//快速删除
	
3.查询命令

	3.1 单条查询
	db.foo.findOne({"_id": 2})
	
	3.2 查询过滤字段
	//只显示 username, email 字段
	db.foo.find({}, {"username" : 1, "email": 1})
	
	//剔除字段
	db.foo.find({}, {"fatal_weakness": 0})
	
	3.3 条件查询
	db.user.find({"age": {"$gte": 18, "$lte": 30}})
	start = new Date("01/01/2007")
	db.users.find({"registered": {"$lt": start}})
	
	
	
	
	
4.更新命令

	4.1 单条更新
	db.foo.update({"_id": 2},{"_id": 9})
	
	4.2 多条更新
	db.foo.update({"name": "liupeng"},{$set:{"name": "haha"}}, false, true)
	
	注: 全部更新 必需配合$set关键字, 最后一位必须是true

	
5.$inc 修改器

	5.1 单条inc
	db.foo.insert({"url":"www.example.com", "pageviews": 53})
	db.foo.update({"url":"www.example.com"}, {$inc:{"pageviews": 1}})
	
	注: 属性不存在时，自动创建，  默认只取得最近一条匹配记录后返回
	
6.$set 修改器

	6.1 单条修改
	db.foo.update({"url":"www.example.com"}, {$set: {"favorite book" : "Green Eggs and Ham"}})
	
	db.foo.update({"url":"www.example.com"}, {$set: {"favorite book" : ["Cat's Cradle","Foundation Trilogy","Ender's Game"]}})
	
	6.2 取消属性
	db.foo.update({"url":"www.example.com"}, {$unset: {"favorite book" : 1}})
	
7.$push 修改器

	7.1 添加数组元素
	db.blog.posts.update({"title":"A blog post"},{"$push":{"comments": {"name":"joe","email":"joe@example.com","content":"nice post."}}})
	
8.$each 修改器

	8.1 添加多个值
	db.blog.posts.update({"title":"A blog post"},{"$push": {"hourly": {"$each": [567.776, 562.790, 559.123]}}})
	

9.$slice 修改器

	9.1 添加指定数量的数组元素
	db.blog.posts.update({"title":"A blog post"},{"$push": {"hourly": {"$each": [00,11,22,33,44,55,66,77,88,99],"$slice": -10}}})
	
	注: 超过10个数组时,前面的元素会舍去，保留最后的10个元素，类似队列
	
10.$sort 修改器

	10.1 添加元素排序后保留
	db.blog.posts.update({"title":"A blog post"},{"$push": {"top10": {"$each": [{"name": "Nightmare on Elm Street", "rating" : 3}, {"name": "Saw", "rating": 100}],"$slice": -10,"$sort":{"rating" : -1}}}})
	
	注: 先添加，在排序，根据排序字段， 保留前10个
	
11.$addToSet 修改器 （避免重复插入）

	1.重复插入（会忽略）
	db.users.insert({"username":"joe","emails":["joe.example.com","joe@gmail.com","joe@yahoo.com"]})
	db.users.update({"username":"joe"}, {"$addToSet": {"emails":"joe@gmail.com"}})

	2.插入不同
	db.users.update({"username":"joe"}, {"$addToSet": {"emails":"joe@hotmail.com"}})	
	
	3.$each 添加多个
	db.users.update({"username":"joe"}, {"$addToSet": {"emails":{"$each": ["joe@php.net","joe.example.com","joe@python.org"]}}})
	
12.$pull 修改器 (移除特定元素)

	1.$pull
	db.lists.insert({"todo" : ["dishes", "laundry", "dry cleaning"]})
	db.lists.update({}, {"$pull" : {"todo" : "laundry"}})
	
13.指定位置的数据修改器
	
	1. 使用数组下标
	db.blog.posts.insert({"content":"内容", comments:[{"comment": "good post", "author": "John", "votes": 0},{"comment" : "i thought it was too short", "author": "Claire", "votes": 3},{"comment" : "free watches", "author" : "Alice", "votes" : -1}]})
	
	db.blog.posts.update({"_id": ObjectId("5593716bf9af3f13adea9c0b")}, {"$inc" : {"comments.0.votes": 1}})
	
	2.不使用下标
	db.blog.posts.update({"comments.author": "John"}, {"$set" : {"comments.$.author": "Jim"}})
	
	
14.upsert更新，更新时如果没有找到文档，则已更新文档为基础，创建新的文档（原子性操作）

	1.e.g.
	db.users.update({"rep": 25}, {$inc : {"rep": 3}}, true)
	
	注: 开启条件是 第三个参数
	
	2. setOnInsert （只在创建时，设置值）
	db.users.update({},{"$setOnInsert" : {"createdAt": new Date()}}, true)
	
	
15.查看更新文档的数量

	1.
	db.count.update({x : 1}, {$inc : {x : 1}}, true, true)
	db.count.update({x : 1}, {$inc : {x : 1}}, true, true)
	db.count.update({x : 1}, {$inc : {x : 1}}, true, true)
	db.count.update({"x":2},{"$set":{"x": 10}}, false, true)
	db.runCommand({getLastError : 1})	
16.$in $or 查询
	1.单键查询	db.raffle.find({"ticket_no" : {"$in" : [725, 542, 390]}})
	如果ticket_no 的值是 725，542， 390 其中一个，满足条件
	2.多键查询	db.raffle.find({"$or" : [{"ticket_no" : 725}, {"winner" : true}]})
	如果ticket_no 是 725， 或者 winner 是true 时， 满足条件
		
	

11 索引 

	11.1 单条
	db.users.ensureIndex({"username" : 1})
	
	11.2 复合索引
	 db.users.ensureIndex({"age": 1, "username": 1})
	 

	


	


	
	