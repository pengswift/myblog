title: vagrant 开发环境配置
date: 2015-07-21 13:34:35
tags: vagrant
---

###1.固定私有IP
	
config.vm.network :private_network, ip: "11.11.11.11"

###2.使用自定义用户名

config.ssh.username = "game1"

###3.指定ssh端口

config.ssh.port = "2222"

###4.使用private key 登录
config.ssh.private_key_path = "~/.ssh/id_rsa"


#ubuntu操作
###1.创建新用户(useradd会自动创建home/game1目录，并拷贝相关文件)

sudo useradd game1
sudo passwd game1

###2.创建组

sudo groupadd docker

###3.添加用户到组

sudo usermod -aG docker game1
//添加到sudo组，使其可使用sudo命令
sudo gpasswd -a game1 sudo

###4.取消game1执行sudo命令需要输入密码， 方便vagrant系统命令

vim /etc/sudoers.d/vagrant

game1 ALL=(ALL) NOPASSWD:ALL

###5.添加ssh公钥到主机
mkdir ~/.ssh
vim ~/.ssh/authorized_keys
写入公钥信息


