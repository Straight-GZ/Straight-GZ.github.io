---
title: 跨域 CORS和JSON
tags:
  - AJAX
---

#### 一、同源策略

1. 同源定义：

- 源： window.Origin 或 location.origin 可以得到当前源
- 源=协议+域名+端口号
- 如果两个 url 协议、域名、端口号完全一致，那么这两个 url 就是同源的
<!-- more -->

2. 同源策略：

- 浏览器规定：如果 JS 运行在源 A，那么就只能获取源 A 的数据，不允许获取源 B 的数据，即不允许跨源
- 目的：保护隐私
- 主要原因：无法区分发送者，如果没有同源策略，任何网站都可以访问别的网站数据(检查 referer)
- 同源策略：不同源的页面中间，不允许互相访问数据

* 同源策略限制访问数据，引用 css、JS 和图片的时候，其实并不知道其内容，只是在引用

#### 二、跨域

浏览器有同源策略限制，不允许一个源里的文档访问另一个源里资源。而跨域就是为了解决同源策略限制。

- CORS：
  跨源资源共享，使用附加的 HTTP 头来告诉浏览器，准许运行在一个源上的 Web 应用访问位于另一不同源选定的资源。使用**Access-Control-Allow-Origin**响应头可以指定一个可以访问资源的 URI

  ```
  if (path === "/friends.json") {
      response.statusCode = 200
      response.setHeader("Content-Type", "text/json;charset=utf-8")
      response.setHeader("Access-Control-Allow-Origin", "http://crystal.com:9999")
      response.write(fs.readFileSync("public/friends.json"))
      response.end()
  }
  ```

- JSONP ：在无法使用 CORS 跨域的情况下，请求 JS 文件，这个 JS 文件会执行一个回调，回调里面包含数据。可以兼容 IE 浏览器实现跨域，但只能发 get 请求，并且无法读取精确的状态码。

  ```
  function jsonp(url) {
    return new Promise((resolve, reject) => {
       const random = "crystalJSONPCallbackName" + Math.random()
       window[random] = (data) => {
         resolve(data)
       }
       const script = document.createElement("script")
       script.src = `${url}?callback=${random}`
       script.onload = () => {  //无法获取状态码，只能知道成功或者失败
         script.remove()
       }
       script.onerror = () => {
         reject()
       }
       document.body.appendChild(script)
     })
   }
   jsonp("http://qq.com:8888/friends.js").then((data) => {
     console.log(data)
   })
   script.onload = () => {
     console.log(window.xxx)
   }
  ```

  ```server.js
  if (path === "/friends.js") {
    if (request.headers["referer"].indexOf("http://crystal.com:9999") === 0) {
      response.statusCode = 200
      response.setHeader("Content-Type", "text/javascript;charset=utf-8")
      const string = `window["{{xxx}}"]({{data}})`
      const data = fs.readFileSync("public/friends.json").toString()
      const string2 = string
        .replace("{{data}}", data)
        .replace("{{xxx}}", query.callback)     //上面的random
      response.write(string2)
      response.end()
    }
  ```
