---
title: dcm4chee
tags: 
 - dcm4chee
---
### 使用Windows Ubuntu双系统

#### 启用ssh

```shell
ajn404@DESKTOP-2GOT61R:/etc/ssh$ service ssh start
mkdir: cannot create directory ‘/run/sshd’: Permission denied
ajn404@DESKTOP-2GOT61R:/etc/ssh$ sudo service ssh start
 * Starting OpenBSD Secure Shell server sshd                                                                       
 sshd: no hostkeys available -- exiting.
                                                                                                              [fail]
ajn404@DESKTOP-2GOT61R:/etc/ssh$ sudo ssh-keygen -A
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519
ajn404@DESKTOP-2GOT61R:/etc/ssh$ sudo service ssh start
 * Starting OpenBSD Secure Shell server sshd                                                                  [ OK ]
ajn404@DESKTOP-2GOT61R:/etc/ssh$
```

### 安装Wildfly

```shell
#更新存储库索引

```
