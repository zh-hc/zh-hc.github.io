---
title: Linux指令积累
date: 2022-07-22 02:54:13.881
updated: 2022-07-28 14:54:35.615
url: /p=33
categories: 
tags: 
- Linux
---

## 用户相关
```bash
# 创建用户
useradd -m 用户名 # 会默认在/home目录下新建同名文件夹
# 配置密码
passwd 用户名
# 登录用户
su - 用户名
# 退出用户
exit
# 删除用户
userdel -r 用户名
cd /home
sudo rm -r 用户名
```

## 服务器测速脚本
**方法一：**
```bash
wget -qO- bench.sh | bash
```
**方法二(推荐)：**
```bash
wget https://raw.github.com/sivel/speedtest-cli/master/speedtest.py
python speedtest.py --share
```