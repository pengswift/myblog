title: upstart-命令备忘
date: 2015-07-14 19:47:03
tags: ubuntu
---

###upstart script 关键字:
	
	1. exec: 执行命令, 在script块中使用
		exec /usr/bin/zip -v
		
	2. script: 脚本块，包括主运行脚本
	   	script
	   		exec /usr/bin/zip /root/upstart.zip /root/upstart/txt
	   	end script
		
	3. pre-start: 脚本块, 在主运行脚本之前执行的脚本
		pre-start script
			exec rm /root/upstart.txt
			exec echo pre-start >> /root/upstart.txt
		end script
		
	4. post-start: 脚本块, 在主运行脚本之后, running状态之前
		post-start script
			exec echo post-start>> /root/upstart.txt
		end script
		
	5. pre-stop: 脚本块, 在执行stop之前
		pre-stop script
			exec echo pre-stop >> /root/upstart.txt
		end script
		
	6. post-stop: 脚本块, 在主运行脚本被杀死之后
		post-stop script
			exex echo post-stop >> /root/upstart.txt
		end script
		
	7. start on: 事件, 启动任务
		start on startup
		
	8. stop on: 事件, 停止任务
		stop on shutdown
		
	9. description: 描述, 信息提示
		desription "This is a upstart testing"
		
	10. author: 描述, 作者信息
		author "liupeng <pengswift.github.io>"
		
	11. version: 描述, 版本信息
		version "0.0.1 dev"
		
	12. respawn: 命令, 设置服务异常停止后自动重启
		respawn
		
	13. respawn limit: 命令, 设置服务异常停止后重启次数及间隔时间
		respawn limit 15 3
		
	14. instance: 定义实例名字, 可以通过命令給任务传参数
		instance $TTY
		exec /sbin/getty -8 38300 $TTY
		
		#通过命令传参数
		~ start mytest $TTY=tty1
		
	15. kill timeout: 命令, 在达到指定时间后, 停止应用
		kill timeout 5
		
	16. normal exit: 命令, 正常退出，不会被respawn重启
		normal exit 0 TERM
		
	17. env: 变量, 设置任务的环境变量
		env PIDFILE=/var/run/myprocess.pid
		
	18. umask: 变量，设置任务的文件权限掩码
		umask 0755
		
	19. nice: 变量， 设置任务的调度优先级
		nice -5
		
	20. limit: 变量， 设置任务的资源限制
		limit nproc 10 10
		
	21. chroot: 变量, 设置任务的根目录
		chroot /var/lib/www/jail
		
	22. chdir: 变量, 设置任务的工作目录
		chdir /var/tmp
		
###upstart 命令

	1. 查看版本
		~ initctl version
		init (upstart 1.12.1)
	
	2. 查看mytest应用状态
		~ initctl list | grep mytest
		
		~ status mytest
		
	3. 启动mytest应用
		~ initctl start mytest
		
		~ start mytest
		
	4. 停止mytest应用
		~ initctl stop mytest
		
		~ stop mytest
		

###upstart封装mytest

	~ vi /etc/init/mytest.conf

	description "mytest"
	author "bsspirit <http://blog.fens.me>"

	env var=bar
	export var

	start on startup
	stop on shutdown

	respawn
	respawn limit 2 5

	console output

	pre-start script
        logger "pre-start: before: var=$var"
        var=pre-start
        export var
        logger "pre-start: after: var=$var"
	end script

	post-start script
        logger "post-start: before: var=$var"
        var=post-start
        export var
        logger "post-start: after: var=$var"
	end script

	script
        logger "script: before: var=$var"
        var=main
        export var
        sleep 60000
        logger "script: after: var=$var"
	end script

	post-stop script
        logger "post-stop: before: var=$var"
        var=post-stop
        export var
        logger "post-stop: after: var=$var"
	end script
	
	
	
	
	测试mytest程序:
	
	启动mytest任务
		~ start mytest
		mytest start/running, process 3257
		
	查看日志跟踪运行状态
		~ tail -f /var/log/syslog

		Jun 14 20:41:13 liupeng-VirtualBox logger: pre-start: before: var=bar
		Jun 14 20:41:13 liupeng-VirtualBox logger: pre-start: after: var=pre-start
		Jun 14 20:41:13 liupeng-VirtualBox logger: script: before: var=bar
		Jun 14 20:41:13 liupeng-VirtualBox logger: post-start: before: var=bar
		Jun 14 20:41:13 liupeng-VirtualBox logger: post-start: after: var=post-start
	
	查看mytest任务状态
		~ status mytest
		mytest start/running, process 3257
		
	查看系统进程，因为在程序中用sleep停止，要通过sleep查询
		~ ps -aux|grep sleep
		root     3220  0.0  0.0   2176   276 ?        S    20:40   0:00 sleep 60000
	
	杀掉sleep进程，mytest自动重启
		kill -9 3220
	
	停止mytest
		stop mytest
		mytest stop/waiting
		
	通过命令传参数，启动mytest
	start mytest var=liupeng