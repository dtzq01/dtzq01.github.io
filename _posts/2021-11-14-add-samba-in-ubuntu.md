---
layout: post
title: "ubuntu下配置samba"
categories: linux service
---
### 安装环境
```
t@t:~$ cat /proc/version
Linux version 4.15.0-162-generic (buildd@lcy01-amd64-007) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #170-Ubuntu SMP Mon Oct 18 11:38:05 UTC 2021
t@t:~$ uname -a
Linux t 4.15.0-162-generic #170-Ubuntu SMP Mon Oct 18 11:38:05 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

```
### 1. 安装samba
```
sudo apt install samba -y
```
### 2. 添加samba用户
```
sudo smbpasswd -a t
```
### 3. 编辑配置文件，指定文件文件
```
sudo vim /etc/samba/smb.conf
```
将如下内容添加到配置文件末尾：
```
[shared]
comment = share folder
browseable = yes
path = /home/t/
create mask = 0700
directory mask = 0700
valid users = t
force user = t
force group = t
public = yes
available = yes
writable = yes
```
### 4. 重启服务，查看samba是否正常
```
sudo systemctl restart smbd.service
```


