title: golang-pprof性能监控
date: 2015-06-27 23:59:47
tags: golang
---

web服务器:

	import (
		_ "net/http/pprof" 
	)
	
服务进程（游戏服务）
	
	import (
		_ "net/http/pprof" 
	)
	
	go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil)) 
	}()
	
	
访问地址: http://localhost:port/debug/pprof/