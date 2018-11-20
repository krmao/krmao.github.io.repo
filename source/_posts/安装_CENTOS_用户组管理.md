---
title: 安装_CENTOS_用户组管理
date: 2018-11-19 18:29:49
categories: [技术]
tags: [安装,CENTOS]
---

```
# 新增用户组
groupadd dev

# 新增用户并添加到用户组
useradd -g dev user_name

# 为新增的用户修改密码
echo "123456" | passwd --stdin user_name

Changing password for user user_name.
passwd: all authentication tokens updated successfully.

# 查看用户所属的组
groups user_name

# 查看用户信息
id user_name

# 从组中删除用户
gpasswd dev -d user_name

Removing user user_name from group dev
gpasswd: user 'user_name' is not a member of 'dev'

# 添加已有用户到组
gpasswd dev -a user_name

Adding user user_name to group dev

# 永久删除用户
userdel user_name
gpasswd dev -d user_name
# 同时删除该用户所有数据
rm -rf /home/user_name

# 给用户添加 sudo 权限(为了方便大佬)
vi /etc/sudoers
# 找到 root    ALL=(ALL)       ALL 这一行,在下面添加  user_name    ALL=(ALL)       ALL 

## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
user_name    ALL=(ALL)       ALL
user_name_1    ALL=(ALL)       ALL
user_name_2    ALL=(ALL)       ALL

wq! 保存即可
```
