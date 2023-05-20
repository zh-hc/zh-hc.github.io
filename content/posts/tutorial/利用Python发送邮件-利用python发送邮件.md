---
title: 利用Python发送邮件
date: 2023-01-23 13:31:45.48
updated: 2023-01-23 13:34:11.997
url: /p=65
categories: 
- tutorial
tags: 
- Python
---

Python有两个内置库：smtplib和email，能够实现邮件功能，smtplib库负责发送邮件，email库负责构造邮件格式和内容。
## 思路
登录 -> 写邮件 -> 发送

## 代码编写
与Python相关的邮件发送库有这几个：
1. smtplib
	- 是关于 SMTP（简单邮件传输协议）的操作模块，在发送邮件的过程中起到服务器之间互相通信的作用。

2. email
	- 简单来说，即服务器之间通信的信息，包括信息头、信息主体等等。

举个简单的例子，当你登录邮箱，写好邮件后点击发送，这部分是由 SMTP 接管；而写邮件、添加附件是由 email 模块控制。

### 导入相关的库和方法
```python
#负责通信
import smtplib
#负责构造文本
from email.mime.text import MIMEText
from email.header import Header
```

### 设置邮箱域名、发件人邮箱、邮箱授权码、收件人邮箱
```python
#邮箱域名，这里用的QQ邮箱发送
host = 'smtp.qq.com'
#port = 25  #或者使用默认的端口号25
#发送者邮箱账号
username = '11********7@qq.com'
#授权码 注意，此处必须填写授权码，不同邮件的获取方法大体相同，参考百度。
password = 'to*************ei'

#接收者邮箱账号,多个接收者，构造list即可。
to_addrs = ['xi.l**@*******.com','xi.hi**@*******.com']

#构造正文，也就是内容
text = '''这是一封Python自动发送的邮件'''
#以下是构造证明，邮件主题、发送者姓名、接收者姓名等。
msg = MIMEText(text,'plain','utf-8')
msg['Subject'] = Header('发给自己的测试邮件')
msg['From'] = Header('Hill Luo')
msg['To'] = Header(','.join(to_addrs))
#如需抄送，可使用Cc进行抄送
# msg['Cc'] = Header(','.join(to_addrs))
```

### 发送邮件
```python
# 创建SMTP对象
server = smtplib.SMTP_SSL(host)
#设置发件人邮箱的域名和端口，端口地址为25
server.connect(host, 465)
#登录邮箱，传递参数1：邮箱地址，参数2：邮箱授权码
server.login(username, password)   
print('开始发送')
#发送邮件，传递参数1：发件人邮箱地址，参数2：收件人邮箱地址，参数3：把邮件内容格式改为strserver.sendmail(username,  to_addrs, msg.as_string())
print('邮件发送成功') 
# 关闭SMTP对象
server.quit()
```

## 注意
一些邮箱登录比如 QQ 邮箱需要 SSL 认证，所以 SMTP 已经不能满足要求，而需要SMTP_SSL，解决办法为：
```python
 #常规方式
 server = smtplib.SMTP()
 #连接到服务器
 server .connect(mail_host,25)  
 # SSL认证方式
 server= smtplib.SMTP_SSL(mail_host)
 #连接到服务器，端口改为465
 server .connect(mail_host,465)

```