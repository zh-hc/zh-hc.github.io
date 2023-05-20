---
title: Clash+EveryProxy实现手机热点VPN分享
date: 2022-07-16 15:19:41.0
updated: 2022-07-20 15:19:48.823
url: /p=15
categories: 
tags: 
---

## 准备
1. 首先下载安装好`Clash for Android`和`Every Proxy`
2. Clash中的VPN配置无误

## 实践
1. 开启手机热点
2. 开启Clash中的VPN
    - 首页 -> 设置 -> 覆写 -> 将`允许来自局域网的连接`改为`已启用`状态
3. 打开Every Proxy
    - 开启HTTP proxy
    - 记录Hosts值(192.168.xxx.xxx)和Port(8080)
4. 进入已经连接了热点的设备，在WiFi设置中将代理设置为手动，IP地址与端口填写上一步记录下的值即可。

## 备注
1. 测试的Clash for Android版本号为`2.5.9.premium`，在OneDrive中已有备份
2. 测试的Every Proxy版本号为`9.2`，在OneDrive中已有备份
3. 测试时间：2022年7月16日