---
layout: post
title: "ubuntu下配置samba"
categories: linux service
tags: samba
---
- [安装环境](#安装环境)
- [FAQ](#faq)
- [1. 添加单目录](#1-添加单目录)
  - [1.1 安装samba](#11-安装samba)
  - [1.2 添加samba用户](#12-添加samba用户)
  - [1.3 编辑配置文件，指定文件文件](#13-编辑配置文件指定文件文件)
  - [1.4 重启服务，查看samba是否正常](#14-重启服务查看samba是否正常)
- [2. 使用Homes配置](#2-使用homes配置)
  - [2.1 安装samba](#21-安装samba)
  - [2.2 编辑配置文件](#22-编辑配置文件)
  - [2.3 添加用户并使能](#23-添加用户并使能)
  - [2.4 重启服务，查看samba是否正常](#24-重启服务查看samba是否正常)

## 安装环境
```
t@t:~$ cat /proc/version
Linux version 4.15.0-162-generic (buildd@lcy01-amd64-007) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #170-Ubuntu SMP Mon Oct 18 11:38:05 UTC 2021
t@t:~$ uname -a
Linux t 4.15.0-162-generic #170-Ubuntu SMP Mon Oct 18 11:38:05 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

```
## FAQ
1. 解决samba访问软连接的问题
```
[global]
…
follow symlinks = yes
wide links = yes
unix extensions = no
```

## 1. 添加单目录
### 1.1 安装samba
```
sudo apt install samba -y
```
### 1.2 添加samba用户
```
sudo smbpasswd -a t
```
### 1.3 编辑配置文件，指定文件文件
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
### 1.4 重启服务，查看samba是否正常
```
sudo systemctl restart smbd.service
```

## 2. 使用Homes配置
### 2.1 安装samba
```
sudo apt install samba -y
```
### 2.2 编辑配置文件
`sudo vim /etc/samba/smb.conf`
```
#======================= Share Definitions =======================

# Un-comment the following (and tweak the other settings below to suit)
# to enable the default home directory shares. This will share each
# user's home directory as \\server\username
[homes]
   comment = Home Directories
   browseable = yes

# By default, the home directories are exported read-only. Change the
# next parameter to 'no' if you want to be able to write to them.
   read only = no

# File creation mask is set to 0700 for security reasons. If you want to
# create files with group=rw permissions, set next parameter to 0775.
   create mask = 0755

# Directory creation mask is set to 0700 for security reasons. If you want to
# create dirs. with group=rw permissions, set next parameter to 0775.
   directory mask = 0755

# By default, \\server\username shares can be connected to by anyone
# with access to the samba server.
# Un-comment the following parameter to make sure that only "username"
# can connect to \\server\username
# This might need tweaking when using external authentication schemes
   valid users = %S
```
### 2.3 添加用户并使能

```
sudo smbpasswd -a $username
sudo smbpasswd -e $username
```

### 2.4 重启服务，查看samba是否正常
```
sudo systemctl restart smbd.service
```



