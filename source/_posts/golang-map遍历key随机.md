title: golang-map遍历key随机
date: 2015-07-07 19:55:42
tags: golang
---

e.g. 
	
	package main
	
	import (
		"fmt"
	)
	
	func main() {
		m := make(map[string]string)
		m["hello"] = "echo hello"
		m["world"] = "echo world"
		m["go"] = "echo go"
		m["is"] = "echo is"
		m["cool"] = "echo cool"
		
		for k, v := range m {
			fmt.Printf("k=%v, v=%v\n", k, v)
		}
	}


修正版

e.g.

	package main
	
	import (
		"fmt"
		"sort"
	)
	
	func main() {
		m := make(map[string]string)
		m["hello"] = "echo hello"
		m["world"] = "echo world"
		m["go"] = "echo go"
		m["is"] = "echo is"
		m["cool"] = "echo cool"
		
		sorted_keys := make([]string, 0)
		for k, _ := range m {
			sorted_keys = append(sorted_keys, k)
		}
		
		sort.Strings(sorted_keys)
		
		for _, k := range sorted_keys {
			fmt.Printf("k=%v, v=%v\n", k, m[k])
		}
	}