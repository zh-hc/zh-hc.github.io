---
title: 利用code-server搭建在线VS Code并实现域名访问
date: 2022-07-25 14:25:25.127
updated: 2022-07-26 04:33:33.305
url: /p=38
categories: 
tags: 
---

## 源码下载
GitHub链接：https://github.com/coder/code-server/releases/
选择对应版本下载压缩包即可，如下图，即为amd架构64位4.5.1的压缩包
![Snipaste_2022-07-25_22-27-12](upload/2022/07/Snipaste_2022-07-25_22-27-12.png)

## 临时运行code-server
### 1. 利用宝塔建站
![Snipaste_2022-07-25_22-32-25](upload/2022/07/Snipaste_2022-07-25_22-32-25.png)

### 2. 上传下载的源码到建站目录并解压
![Snipaste_2022-07-25_22-35-17](upload/2022/07/Snipaste_2022-07-25_22-35-17.png)

### 3. 进入终端
```bash
# 直接启动 以8082端口为例 可以自定义端口
./bin/code-server --host 0.0.0.0 --port 8082
```
运行结果如图所示：
![image](upload/2022/07/image.png)

### 4. 访问测试
进入http://IP:8082 如图所示 出现输入密码的界面 说明已经初始化完成
![image-1658760062165](upload/2022/07/image-1658760062165.png)

### 5. 配置密码等项目
在第一次开启code-server时，会生成一个配置文件
路径：`~/.config/code-server/config.yaml`
![image-1658760228509](upload/2022/07/image-1658760228509.png)
> 我们可以将host改为0.0.0.0以供外网访问
![image-1658760325860](upload/2022/07/image-1658760325860.png)
> 此时我们重新启动终端，即可通过自己设置的密码进行登陆了

## 转为持久化运行code-server
### 1. 将code-server转为持久化运行
我们知道，目前的运行需要依靠不关闭终端来实现，我们需要借助宝塔的Supervisor管理器插件实现
![image-1658760941599](upload/2022/07/image-1658760941599.png)

> 到此为止，我们一直是用的IP加端口号的形式访问，接下来配置域名以及https

### 2. 配置域名访问
使用宝塔添加反向代理
![image-1658761117471](upload/2022/07/image-1658761117471.png)
此时就可以使用域名访问了，但是可能会出现如下报错：
Error: WebSocket close with status code 1006
![image-1658761206145](upload/2022/07/image-1658761206145.png)
这是由于Nginx没有使用WebSocket代理
解决方法：我们进入反向代理的配置文件，添加以下代码即可
```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection upgrade;
proxy_set_header Accept-Encoding gzip;
```
![image-1658761347209](upload/2022/07/image-1658761347209.png)

### 3. 配置https访问
使用宝塔中的Let's Encrypt免费申请即可，注意需要先关闭反向代理
申请成功后打开反向代理即可

### 4. 配置域名的泛解析
首先添加泛解析域名
![image-1658795849164](upload/2022/07/image-1658795849164.png)
使用dnspod的id和密钥申请泛解析域名的https证书
泛解析参考下一篇文章。

## 问题
### https无法访问的问题
好像是宝塔443放行有问题
删除443端口放行后重新放行一次即可

### 安装了中文插件却还是英文
重启code-server即可变为中文

> Enjoy It!