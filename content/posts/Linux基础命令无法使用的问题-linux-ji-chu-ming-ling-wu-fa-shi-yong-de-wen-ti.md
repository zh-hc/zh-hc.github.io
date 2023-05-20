---
title: Linux基础命令无法使用的问题
date: 2022-07-15 15:17:40.0
updated: 2022-07-20 15:17:54.179
url: /p=11
categories: 
tags: 
---

## 报错概览
![1657848824703](upload/2022/07/1657848824703.png)

## 临时解决方法
在命令行中输入如下即可临时解决问题，但是当重启服务器或者开启新的命令行窗口时需要重新执行才可生效
```bash
export PATH=/bin:/usr/bin:$PATH
```

## 永久设置 - 经测试无效
```bash
# 首先进入编辑系统环境变量文件
vim /etc/profile

# 然后将临时解决办法粘贴到最后一行
# export PATH=/bin:/usr/bin:$PATH

# 立即生效
source /etc/profile
```