---
title: Dashy - 主页导航
date: 2022-07-02 15:16:18.0
updated: 2022-07-20 15:16:30.139
url: /p=10
categories: 
tags: 
---

## 0. 简介
通过Vercel+本地环境，实现免费网页版主页导航
注意：由于网络原因，可能会导致`网站打开速度缓慢`或者`网站报错`的问题，推荐使用国内服务器直接搭建。
文档地址：[Dashy](https://dashy.to/docs)

## 1. 方法1：使用自己的服务器搭建
fork一份代码到自己的仓库：
https://github.com/lissy93/dashy

## 1. 方法2：一键部署到Vercel
https://vercel.com/new/project?template=https://github.com/lissy93/dashy

## 2. 将该仓库拉取到服务器环境/本地
`git clone git@github.com:你的GitHub用户名/dashy.git`

## 3. 本地运行调试

### 3.1 初次运行
```bash
yarn # Install dependencies
yarn build  # Build the app
yarn start   # Start the app
```

### 3.2 修改配置文件后运行
```bash
yarn build && yarn start  # Build the app and Start the app
```

## end
push到github即可自动将在线版更新为本地修改好的版本
