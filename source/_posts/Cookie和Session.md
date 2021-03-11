---
title: Cookie和Session
tags:
  - HTTP
---

### 1、Cookie

- 概述：

  HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。
  通俗的说，Cookie 是服务器下发给浏览器的一段字符串
  浏览器必须保存这个 Cookie(除非用户删除)
  之后发起相同二级域名请求，浏览器必须附上 Cookie[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)
  <!-- more -->

* 创建 Cookie
  ```
  Set-Cookie: <cookie 名>=<cookie 值>
  //例
  response.setHeader("Set-Cookie", `session_id=${random};HttpOnly`);
  ```

### 2、Session

session 数据放在服务器上,一般由 cookie 保存 session 的 id，保存在 Cookie 里的 session id 是独一无二的随机生成的，服务器通过 Cookie 保存的 session id 获取数据

### 3、两者区别

1. 存储位置：cookie 数据存放在客户的浏览器上，session 数据放在服务器上。

2. 安全性：cookie 不是很安全，别人可以分析存放在本地的 COOKIE 并进行篡改
3. 限制：单个 cookie 保存的数据不能超过 4K，很多浏览器都限制一个站点最多保存 20 个 cookie。

简单实现

```
  if (path === "/sign_in" && method === "POST") {
    const array = [];
    const users = JSON.parse(fs.readFileSync("./db/users.json").toString());
    response.setHeader("Content-Type", "text/html,charset=UTF-8");
    request.on("data", (chunk) => {
      array.push(chunk);
    });
    request.on("end", () => {
      const string = Buffer.concat(array).toString();
      const obj = JSON.parse(string);
      const user = users.find(
        (user) => user.name === obj.name && user.password === obj.password
      );
      if (user === undefined) {
        response.statusCode = 400;
        response.setHeader("Content-Type", "text/json;charset=utf-8");
        response.end();
      } else {
        response.statusCode = 200;
        const random = Math.random();
        session[random] = { user_id: user.id };
        fs.writeFileSync("./session.json", JSON.stringify(session));
        response.setHeader("Set-Cookie", `session_id=${random};HttpOnly`);
        response.end();
      }
    });
  } else if (path === "/home.html") {
    const cookie = request.headers["cookie"];
    let sessionId;

    try {
      sessionId = cookie
        .split(";")
        .filter((s) => s.indexOf("session_id=") >= 0)[0]
        .split("=")[1];
      console.log(sessionId);
    } catch (error) {}
    if (sessionId && session[sessionId]) {
      const userId = session[sessionId].user_id;
      console.log(userId);
      const homeHtml = fs.readFileSync("./public/home.html").toString();
      const users = JSON.parse(fs.readFileSync("./db/users.json").toString());
      const user = users.find((user) => user.id === userId);
      let string;
      if (user) {
        string = homeHtml
          .replace("{{loginStatus}}", "已登录")
          .replace("{{userName}}", user.name);
      } else {
        string = homeHtml
          .replace("{{loginStatus}}", "未登录")
          .replace("{{userName}}", "");
      }
      response.write(string);
      response.end();
    } else {
      const homeHtml = fs.readFileSync("./public/home.html").toString();
      const string = homeHtml
        .replace("{{loginStatus}}", "未登录")
        .replace("{{userName}}", "");
      response.write(string);
    }
  }
```

#### Cookie V.S. LocalStorage V.S. SessionStorage V.S. Session

- Cookie V.S. LocalStorage

  主要区别是 Cookie 会被发送到服务器，而 LocalStorage 不会
  Cookie 一般最大 4k，LocalStorage 可以用 5Mb 甚至 10Mb（各浏览器不同）

* LocalStorage V.S. SessionStorage

  LocalStorage 一般不会自动过期（除非用户手动清除），而 SessionStorage 在回话结束时过期（如关闭浏览器）

* Cookie V.S. Session

  Cookie 存在浏览器的文件里，Session 存在服务器的文件里
  Session 是基于 Cookie 实现的，具体做法就是把 SessionID 存在 Cookie 里
