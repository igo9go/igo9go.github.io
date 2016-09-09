---
title: expect脚本使用
date: 2016-09-06 14:12:09
tags: 自动化
categories: linux
---
#expect自动化远程登录脚本

首先要安装 expect 脚本程序，不过在 Macbook 中自带了。比如 CentOS 的机器，可以使用 yum install expect 的方式安装。

###简单模式

```
#!/usr/bin/expect -f   
set timeout 5   
spawn ssh root@192.168.0.1
expect "*assword*"   
send "root\r"   
expect "#"   
send "ifconfig \r"   
expect eof

```
spawn: 启动新的进程
expect:从进程接受字符串
send：用于向进程发送字符串
interact:容许用户交互

spawn 后面就是要执行的 shell 命令，expect 是捕获要等待输入的字符，send 是自动输入的内容，注意要 “\r” 表示换行以确认输入。

###使用变量
```
#!/usr/bin/expect -f
set port 22
set user root
set host 192.168.0.12
set password root
set timeout -1
spawn ssh -D $port $user@$host "ifconfig"
expect {  
"*yes/no" { send "yes\r"; exp_continue}  
"*assword:" { send "$password\r" }  
}  
expect "*#*"  
send "ifconfig > /home/cfg \r"
send "exit\r"
}
```

###通过读取配置文件获取变量：

配置文件
192.168.0.1 root
192.168.0.2 root

登陆脚本

<pre>
#!/usr/bin/expect -f
set f [open ./ip r]  
while { [gets $f line ]>=0 } {       
set ip [lindex $line 0]     
set pwd [lindex $line 1]
spawn ssh $ip
expect  "*password:" { send "$pwd\r" }
expect "#"
send "ifconfig \r"
send "exit\r"
interact
}
</pre>







