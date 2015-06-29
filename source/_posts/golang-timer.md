title: golang-timer
date: 2015-06-29 13:52:34
tags: golang
---

### Timer
用途: 定时器

	e.g.	
	
	方式1:

		time.AfterFunc(5*time.Second, func() {
        	fmt.Println("expired")
    	})
	
	方式2:
	
		timer := time.NewTimer(5 * time.Second)
    	<-timer.C
    	fmt.Println("expired")
    	
    方式3:
    	
    	<-time.After(5 * time.Second)
    	fmt.Println("expired")
	
