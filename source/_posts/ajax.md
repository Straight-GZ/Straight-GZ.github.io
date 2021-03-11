---
title: 跨域 CORS和JSON
tags:
  - AJAX
---

1. 概述

AJAX：Asynchronous JavaScript + XML（）

- AJAX 是浏览器上的功能，浏览器可以发请求，收响应
- 浏览器在 window 上加了一个 XMLHttpRequest 函数

* 用这个构造函数（类）可以构造出一个对象
JS 通过它实现发请求，收响应
<!-- more -->

2. 使用

- JS 代码
  ```
  const request = new XMLHttpRequest()
  request.open("GET", "/style.css")
  request.onreadystatechange = () => {
      console.log(request.readyState)
      if (request.readyState === 4) {
          if (request.status >= 200 && request.status < 300) {
               console.log("下载完成了")
              const style = document.createElement("style")
              style.innerHTML = request.response
               document.head.appendChild(style)
      } else {
          alert("加载 css 失败")
      }
  }
  }
  request.send()
  ```

* 服务端：

  ```
    //server.js
    var http = require("http")
    var fs = require("fs")
    var url = require("url")
    var port = process.argv[2]

    if (!port) {
    console.log("请指定端口号好不啦？\nnode server.js 8888 这样不会吗？")
    process.exit(1)
    }

    var server = http.createServer(function (request, response) {
    var parsedUrl = url.parse(request.url, true)
    var pathWithQuery = request.url
    var queryString = ""
    if (pathWithQuery.indexOf("?") >= 0) {
        queryString = pathWithQuery.substring(pathWithQuery.indexOf("?"))
    }
    var path = parsedUrl.pathname
    var query = parsedUrl.query
    var method = request.method

    console.log("有个傻子发请求过来啦！路径（带查询参数）为：" + pathWithQuery)

    if (path === "/index.html") {
        response.statusCode = 200
        response.setHeader("Content-Type", "text/html;charset=utf-8")
        let string = fs.readFileSync("public/index.html").toString()
        const page1 = fs.readFileSync("db/page1.json").toString()
        const array = JSON.parse(page1)
        const result = array.map((item) => `<li>${item.id}</li>`).join("")
        string = string.replace("{{page1}}", `<ul id="aa">${result}</ul>`)
        response.write(string)
        response.end()
    } else {
        response.statusCode = 404
        response.setHeader("Content-Type", "text/css;charset=utf-8")
        response.write(`你访问的页面不存在`)
        response.end()
    }
    })

    server.listen(port)
    console.log(
    "监听 " +
        port +
        " 成功\n请用在空中转体720度然后用电饭煲打开 http://localhost:" +
        port
    )
  ```
