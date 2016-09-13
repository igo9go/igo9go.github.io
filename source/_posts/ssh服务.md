---
title: ssh服务
date: 2016-09-13 09:14:49
tags: ssh
categories: linux
---

1. 概述
    SSH是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议，利用SSH协议可以有效防止远程管理过程中的信息泄露问题。SSH是Secure Shell的缩写，是建立在应用层和传输层基础上的安全协议。
2. SSH服务的功能
    * SSH可有效地提供防止DNS欺骗以及IP欺骗
    * 压缩传输数据
    * SSH提供了多种传输方式
    * SSH构建Socket5代理
3. SSH常用配置
    linux系统，SSH配置文件在 /etc/ssh/sshd_config
    
    常用配置说明:
    
    Port	 22	#SSH端口设置，这里默认使用的是22端口
    Protocol	2，1		#选择SSH协议版本
    ListenAddress	 0.0.0.0		#监听的网卡IP
    PermitRootLogin	 no	#是否允许root登入，默认是允许的
    PasswordAuthentication yes #是否开启密码验证
    PermitEmptyPasswords no	#是否允许密码为空
    PrintMotd no					#登入后是否显示一些信息，如上次登入时间及地点等。
    PrintLastLog yes			#显示上次登入的信息
    KeepAlive yes					#发送KeepAlive信息给客户端
    MaxStartups 10				#允许尚未登入的联机画面数
    DenyUsers *					#禁止用户登录，*表示所有用户
    AllowUsers *					#允许用户登录
4. SSH的认证方式与访问策略
    * 基于口令的认证方式
        ssh options username@hostname `command`
    * 基于秘钥的认证方式
        ssh-keygen 生成公钥与秘钥，讲客户端公钥加入到服务端
        cat id_rsa.pub >> ~/.ssh/authorized_keys
        ssh 目录权限700  authorized_keys权限600
    * 限制用户连接
        可以修改ssh服务端配置文件 /etc/ssh/sshd_config
        
        DenyUsers 		test	#禁止test用户登入
        AllowUsers	 	test	#允许test用户登入
        DenyGroups 	   test	#禁止test群组登入
        AllowUsers	 	test	#允许test群组登入
    * 限制ip连接SSH
        基于IP地址的访问策略有两种方式实现：
        - iptables防火墙
        
        	iptables  -A INPUT -p tcp --dport 22 -s 192.168.0.10/32 -j ACCEPT
        	iptables  -A INPUT -p tcp --dport 22 -j DROP
        - TCP Wrappers
        
        	/etc/hosts.allow
        	sshd:192.168.0.10/255.255.255.255
        	
        	/etc/hosts.deny
        	sshd:ALL 或  sshd:ALL EXCEPT 192.168.0.10
        	TCP Wrappers只支持长格式掩码，不能用192.168.0.0/24

            




