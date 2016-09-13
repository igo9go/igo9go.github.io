---
title: 配置ssh密钥认证自动登录
date: 2016-09-13 09:04:08
tags: ssh
categories: linux
---
#配置ssh密钥认证自动登录
###在客户端来看，SSH提供两种级别的安全验证。
1. 在本地机器创建公钥
 `ssh-keygen -t rsa -C  'your email@domain.com'`
 -t 指定密钥类型，默认即 rsa ，可以省略
 -C 设置注释文字，比如你的邮箱
2. 将公钥复制到ssh服务器
    将前一步骤生成的公钥~/id_rsa.pub文件，复制到ssh服务器对应用户下的~/.ssh/authorized_keys文件,可以有多种方式，这里只介绍常用的三种。
    * [适用于osx系统]使用ssh-copy-id-for-OSX工具将公钥复制至ssh服务器
    
        ```
        brew install ssh-copy-id
        
        ssh-copy-id username@hostname  #将username和hostname替换为你的ssh服务器用户名和IP
```
    * 当ssh服务器username用户目录下尚未有.ssh目录时使用此方式
        `cat ~/.ssh/id_rsa.pub | ssh username@hostname "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"`
    * 通用方式
        ```
        scp ~/.ssh/id_rsa.pub username@hostname:~/ #将公钥文件复制至ssh服务器
        ssh username@hostname #使用用户名和密码方式登录至ssh服务器
        mkdir .ssh  #若.ssh目录已存在，可省略此步
        cat id_rsa.pub >> .ssh/authorized_keys  #将公钥文件id_rsa.pub文件内容追加到     authorized_keys文件
```
3. 快捷登录
    完成以上步骤后，即可使用以下命令直接登录ssh服务器
    `ssh username@hostname #将username替换为你的ssh服务器用户名，hostname替换为服务器的ip`
    如果你本地终端使用的是zsh，那就太简单不过了，直接给zsh添加一条别名
    ```
    echo "alias ssh-to-username='ssh username@hostname'" >> ~/.zshrc #将username和        hostname替换为你的服务器信息
    source ~/.zshrc   #重新加载更改后的zshrc文件
    ssh-to-username   #使用别名，一条命令即可登录你的ssh服务器
    ```


