title: docker-命令备忘
date: 2015-07-14 19:00:37
tags: docker
---

###docker 命令

	1. 创建容器
		docker run -i -t ubuntu /bin/base
		
	2. 创建并设置容器别名
		docker run --name bob_the_container -i -t ubuntu /bin/bash
		
	3. 启动容器
		docker start bob_the_container
		
		docker start aa3f365f044e
		
		
	4. 重启容器
		docker restart bob_the_container
		
	5. attach到容器
		docker attach bot_the_container
		
	6. 创建守护式容器
		docker run --name daemon_dave -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
		
	7. 查看容器日志
		docker logs daemon_dave
		
	8. 监控容器日志
		docker logs -f daemon_dave
		docker logs --tail 10 daemon_dave
		docker logs --tail 0 -f daemon_dave
		
	9. 查看容器内进程
		docker top daemon_dave
		
		
	10. 下载镜像
		docker pull ubuntu:latest
		
	11. 查看已下载的镜像
		docker images ubuntu
		
	12. 查找可用的镜像
		docker search puppet
		
	13. 创建镜像
		docker commit 4aab3ce3cb76 jamtur01/apache2
		
	14. 创建并添加提交信息 (日志， 作者， tag)
		docker commit -m="A new custom image" --author="liupeng" 4aab3ce3cb76 jamtur01/apache2:webserver
		
	15. 查看镜像详细信息
		docker inspect jamtur01/apache2:webserver
		
	16. 通过Dockerfile 创建镜像
		＃ Version: 0.0.1
		FROM ubuntu:14.04
		MAINTRAINER liupeng "pengswift.github.io"
		RUN apt-get update
		RUN ap-get install -y nginx
		RUN echo "Hi, I am in your container"
			>/usr/share/nginx/html/index.html
		EXPOSE 80
		
		通过本地Dockerfile
		docker build -t="jamtur01/static_web:v1" .
		
		通过git仓库
		docker build -t="jamtur01/static_web:v1" git@github.com:jamtur01/docker-static_web
		
	17. 不使用缓存创建镜像
		docker build --no-cache -t="jamtur01/static_web"		
	18. 查看镜像的创建历史
		docker history training/postgres
		
	19. 启动容器，指定容器端口
		docker run -d -p 80 --name static_web jamtur01/static_web nginx -g "daemon off;"
		
	20. 查看容器端口在宿主上的映射端口
		docker port 6752b94bb5c0 80
		
	21. 启动容器，指定宿主和容器端口
		docker run -d -p 8080:80 --name static_web jamtur01/static_web nginx -g "daemon off;"
		
	22. 启动容器，指定宿主和容器端口, 限制ip访问
		docker run -d -p 127.0.0.1:8080:80 --name static_web jamtur01/static_web nginx -g "daemon off;"
	
	23. 启动容器，使用Dockerfile 默认端口
		docker run -d -P --name static_web jamtur01/static_web nginx -g "daemon off;"
		
	