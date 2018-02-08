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
debug1: Connection established.
debug1: identity file /home/chengsh/.ssh/id_rsa type 1
debug1: Checking blacklist file /usr/share/ssh/blacklist.RSA-2048
debug1: Checking blacklist file /etc/ssh/blacklist.RSA-2048
debug1: identity file /home/chengsh/.ssh/id_rsa-cert type -1
debug1: identity file /home/chengsh/.ssh/id_dsa type -1
debug1: identity file /home/chengsh/.ssh/id_dsa-cert type -1
debug1: identity file /home/chengsh/.ssh/id_ecdsa type -1
debug1: identity file /home/chengsh/.ssh/id_ecdsa-cert type -1
debug1: Remote protocol version 2.0, remote software version OpenSSH_5.5p1 Debian-6+squeeze1+github12
debug1: match: OpenSSH_5.5p1 Debian-6+squeeze1+github12 pat OpenSSH*
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_5.9p1 Debian-5ubuntu1.1
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: server->client aes128-ctr hmac-md5 none
debug1: kex: client->server aes128-ctr hmac-md5 none
debug1: SSH2_MSG_KEX_DH_GEX_REQUEST(1024<1024<8192) sent
debug1: expecting SSH2_MSG_KEX_DH_GEX_GROUP
debug1: SSH2_MSG_KEX_DH_GEX_INIT sent
debug1: expecting SSH2_MSG_KEX_DH_GEX_REPLY
debug1: Server host key: RSA 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in /home/chengsh/.ssh/known_hosts:1
debug1: ssh_rsa_verify: signature correct
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: Roaming not allowed by server
debug1: SSH2_MSG_SERVICE_REQUEST sent
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering RSA public key: /home/chengsh/.ssh/id_rsa
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
