---
layout: post
title: Github SSH 不能验证问题
date: 2017-06-21 19:14:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: github.jpg # Add image post (optional)
tags: [Github, SSH]
---
### 问题描述：

连接github时，公钥出现问题。执行 
``` bash
$ssh -vT git@github.com
```
出现如下提示：
``` bash
OpenSSH_5.9p1 Debian-5ubuntu1.1, OpenSSL 1.0.1 14 Mar 2012
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to http://github.com [204.232.175.90] port 22.
...
debug1: Server accepts key: pkalg ssh-rsa blen 279
Agent admitted failure to sign using the key.
debug1: Trying private key: /home/chengsh/.ssh/id_dsa
debug1: Trying private key: /home/chengsh/.ssh/id_ecdsa
debug1: No more authentication methods to try.
Permission denied (publickey).
```
### 解决方法：

生成RSA公钥并将公钥添加到GitHub的SSH Keys后：
```shell
$eval "$(ssh-agent) -s"
$ssh-add
$ssh-add -l -E md5
```
