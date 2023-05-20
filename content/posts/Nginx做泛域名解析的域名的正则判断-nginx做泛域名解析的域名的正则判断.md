---
title: Nginx做泛域名解析的域名的正则判断
date: 2022-07-25 23:53:54.562
updated: 2022-07-26 04:42:22.619
url: /p=39
categories: 
tags: 
---

## 一、 泛域名的解析与使用基础
1. 域名的泛域名解析 
    - 作用：可以让域名支持无限的子域名(这也是泛域名解析最大的用途)。
    - 泛域名可以用根域名，也可以用二级域名
    - 泛域名解析，需要一台有独立IP的服务器
    - 根域名的泛域名解析，添加`*`的A记录， 例如：`*.domain.com`
    - 二级域名的泛域名解析， 添加`*.二级名称`，例如：`*.test.domain.com`
2. nginx可以直接做泛域名解析
    - 直接设置serever中的server_name
        ```
        server {
            listen       80;
            server_name *.aaa.bbbb.com

            location / {

               root   /data/www/test/;
               index  index index.html;
            }
        }
        ```
任何一个x.aaa.bbbb.com都可以访问

## 二、 泛域名也有规则，比如字符的长度，字符的规则，如何设置规则呢？
1. 需要使用nginx的正则
    - `^`：匹配字符串的开始位置；
    - `$`：匹配字符串的结束位置；
    - `.`和`*`：`.`匹配任意字符，`*`匹配数量0到正无穷；
    - `\`：将后面接着的字符标记为一个特殊字符或者一个原义字符或一个向后引用；
    - `(值1|值2|值3|值4)`：`或匹配`模式，例：(jpg|gif|png|bmp)匹配jpg或gif或png或bmp
    - `i`：不区分大小写
    - `*~`：区分大小写匹配
    - `~*` ：不区分大小写匹配
    - `!`和`!*`：分别为区分大小写不匹配及不区分大小写不匹配
    - `(pattern)`：匹配括号内的pattern
    - `-`：匹配前面字符串一次或者多次
    - nginx是支持`[a-zA-Z]`和`{n,m}`的，但是{}与conf的冲突，所以，匹配规则一定要用`""`包含起来
2. 需要使用nginx的判断if
    ```
    # 示例一
    if ($host !~* "^[a-zA-Z0-9_]{3,10}\.test\.domain\.com$") {
          return 444;
     }
    # 示例二    
    if ($host ~* ^newhouse\.(.+)?\.house365\.com) {
         set $host_city $1;
         rewrite ^(.*)$ http://$host_city.house365.com$1 permanent;
    }
    ```
3. 需要使用nginx的变量，例如，下面的泛域名解析
    ```
    server_name ~^(?<subdomain>.+)\.test\.domain\.com$;
    if ($subdomain !~* "^[a-zA-Z0-9]{3,10}$") {
      return 444;
    }
    ```
4. 需要使用nginx的return
    - 可以直接通过return code 来返回http的状态码

## 三、为泛域名解析设置正则判断的方法
1. 不设置severname 直接判断$host
    ```
    server {
        listen       80;
        if ($host !~* "^[a-zA-Z0-9_]{3,10}\.test\.domain\.com$") {
            return 444;
        }
    
        location / {
    
           root   /data/www/test/;
           index  index index.html;
        }
    }
    ```
2. 设置server_name取出变化的部分，再判断
    ```
    server {
        listen       80;
        server_name ~^(?<subdomain>.+)\.test\.domain\.com$;
        if ($subdomain !~* "^[a-zA-Z0-9]{3,10}$") {
            return 444;
        }
    
        location / {
    
           root   /data/www/test/;
           index  index index.html;
        }
    }
    ```

## 四、nginx的正则使用的总结
1. 字符长度判断`{n,m}` 需要给整个规则加上`""`
    `实现1的时候，因为没有看过网上有加双引号的案例，所以不敢加 $host !~* ^[a-zA-Z0-9_]{3,10}\.test\.domain\.com$ 一直报错，一度怀疑nginx是否支持长度的判断`
2. 在severname中一直无法，实现直接判断长度，所以才有2的变种实现
    `server_name ~ "^[a-zA-Z0-9_]{3,6}\.test\.domain\.com$";一直报server_name不是以;为结束的错误`
    
## 五、参考链接
https://www.cnblogs.com/qumogu/p/13713936.html