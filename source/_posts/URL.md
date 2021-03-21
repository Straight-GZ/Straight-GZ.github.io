---
title: URL
tags:
  - CSS
  - 动画
---

**URL（统一资源定位符 Uniform Resource Locator）**

包括：**协议+域名或 IP+端口号+路径+查询字符串+锚点 **
<br>

  <!-- more -->

## 一、协议 HTTP

基于 TCP 和 IP 两个协议

**curl 命令**

用 curl 可以发 HTTP 请求

- curl -v http:<span>//baidu.com</span>

- curl -s -v https://<span>www.<span>baidu.com</span>

注：

- url 会被 curl 工具重写，先请求 DNS 获得 IP
- 先进行 TCP 链接，TCP 连接成功后，发送 HTTP 请求
- 请求、响应结束后，关闭 TCP 连接（看不出来），真正结束。
  <br>

具体内容，后续详细介绍

## 二、域名

### 1、IP(Internet Protocol)网际互联协议

IP 主要约定两件事：

- 如何定位一台设备
- 如何封装数据报文，以跟其他设备交流

1、外网 IP 和内网 IP

- 租用宽带，连接路由器，就会有一个**外网 IP**（重启路由器，可能会重新分配）
- 路由器创建内网，内网中的设备使用内网 IP，一般格式是 192.168.X.X

2、路由器

- 路由器有一个内网 IP、一个外网 IP
- 内网中的设备可以互相访问，但不能直接访问外网，需经过路由器中转
- 外网中的设备可以互相访问，但无法访问你的内网，访问也必须结果路由器
- 内网外网互相隔绝，唯一联通的点就是路由器，也被叫做**网关**

特殊的 IP：

- 127.0.0.1 表示自己

- localhost 通过 hosts 指定为自己（可以编辑 hosts 自定义）

- 0.0.0.0 不表示任何设备

### 2、域名

- 域名就是对 IP 的别称
  <br>
  ping 命令 `ping baidu.com` （对应百度的 IP）

  一个域名可以对应不同的 IP，均衡负载

  一个 IP 可以对应不同域名，共享主机

- 域名和 IP 是如何对应的呢？ DNS（**D**omain **N**ame **S**ystem）

  过程：

  1. 当你输入域名，浏览器会向网络运营商的**DNS 服务器**询问对应的 IP（nslookup 命令）
     <br>
     nslookup 命令 `nslookup baidu.com` 得到百度的 IP 地址
  2. 浏览器向对应的 IP 的 **80/443 端口** 发送请求
  3. 请求内容是查看域名首页。

  服务器默认**80 端口**提供 HTTP 服务，**443 端口**提供 https 服务（开发者工具可以看到具体的端口）

- 域名级别：

顶级域名：.com

二级域名：xxx.com 俗称一级域名

三级域名：www.<span>xxx.com</span> 俗称二级域名 （是 xxx.com 的子域名)

- 请求不同的页面，使用**路径**

  https://d<span>eveloper.mozilla.org/zh-CN/docs/Web/HTML<span>

  https://d<span>eveloper.mozilla.org/zh-CN/docs/Web/CSS<span>

- 同一个页面不同内容，使用**查询参数**

  https://www.baidu.com/s?wd=hi （&pn=10 可以确定页数)

  https://www.baidu.com/s?wd=hello

- 同一个内容，不同位置 使用**锚点**

  https://d<span>eveloper.mozilla.org/zh-CN/docs/Web/CSS#参考书<span>

  https://d<span>eveloper.mozilla.org/zh-CN/docs/Web/CSS#教程<span>

  锚点看起来有中文，其实不支持中文，上述链接会变成

  https://d<span>eveloper.mozilla.org/zh-CN/docs/Web/CSS#%E5%8F%82%E8%80%83%E4%B9%A6<span>

  https://d<span>eveloper.mozilla.org/zh-CN/docs/Web/CSS#%E6%95%99%E7%A8%8B<span>

  锚点无法在 Network 面板看到，因为锚点不会传给服务器

  ## 三、端口 Port

一台机器可以提供不同服务

- HTTP 服务最好使用 80 端口
- https 服务使用 443 端口
- ftp 服务使用 21 端口
- 一共 65535 个端口

端口使用规则：

- 0~1023 号端口是留给系统使用的端口，只有拥有管理员权限才能使用
- 其他端口普通用户可以使用，如 http-server 默认使用 8080 端口（http-server -c -1 -p 1234 把端口改为 1234）
- 一个端口如果被占用，只能换一个端口

**端口和 IP 缺一不可**

## 四、URL

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a6d346d2405844818d3e439fe98b4fad~tplv-k3u1fbpfcp-watermark.webp)
**协议+域名或 IP+端口号+路径+查询字符串+锚点 **
<br>
https 端口为默认为 443；
