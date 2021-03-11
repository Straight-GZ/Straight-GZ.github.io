---
title: HTML常用标签
tags:
  - HTML
  - 布局
---

#### 一、a 标签

1. 属性：

- href：

  取值：

  1. 网址：

  - https://www.baidu.com
  - http://www.baidu.com
  - //www.baidu.com（用这个就可以了）

  2. 路径：

  - /a/b/c 和 a/b/c：（绝对路径和相对路径，使用 http-server 效果相同）
  - index.html 以及./index.html

  3. 伪协议
  <!-- more -->

  - javascript:代码（"javascript:;"可用于实现 点击没有任何操作)
  - mailto:邮箱
  - tel：手机号

  4. id

  - href=#xxx 锚点链接

- target：

  1. 内置名字

  - \_blank：新窗口打开。

  - \_top:

    顶层窗口打开。如果当前窗口就是顶层窗口，这个值等同于`_self`

  - \_parent：

    上层窗口打开，这通常用于从父窗口打开的子窗口，或者`<iframe>`里面的链接。如果当前窗口没有上层窗口，这个值等同于`_self`

  - \_self：当前窗口打开，这是默认值。

  2. 程序员命名

  - window 的 name

    两个页面都在同一个名叫 test 的窗口打开

    ```
    <p><a href="http://foo.com" target="test">foo</a></p>
    <p><a href="http://bar.com" target="test">bar</a></p>
    ```

  - iframe 的 name

- download：

  1. 作用：不是打开页面，而是下载页面
  2. 问题：不是有浏览器都支持

- rel=noopener：

2. 作用：

- 跳转到外部页面
- 跳转到内部页面
- 跳转到邮箱或者号码

#### 二、table 标签：表格

相关的标签：

- table：表格

- thead：表头

- tbody：主体

- tfoot：表格末尾

- tr：行

- td：数据单元格

- th：表头，标题单元格

示例：

```
<table>
      <thead>
        <tr>
          <th>学号</th>
          <th>姓名</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>001</td>
          <td>张三</td>
        </tr>

        <tr>
          <td>002</td>
          <td>李四</td>
        </tr>
      </tbody>
      <tfoot></tfoot>
    </table>
```

相关样式：

table-layout

border-collapse

border-spacing

#### 三、img 标签：

作用：

发出 get 请求，展示一张图片

属性：

- src：指定图片网址

  `<img src="foo.jpg">`

- Alt:图片说明。加载失败显示。

  `<img src="foo.jpg" alt="示例图片">`

- height、width：指定高度、宽度 只指定一个，另一个自适应

事件：

onload/onerror （加载成功/加载失败）

响应式：

`max-width:100%` 最大宽度为 100%

#### 四、form：表单标签

作用：发 get 或 post 的请求，然后刷新页面

属性：

action：服务器接收数据的 url

autocomplete：是否自动填充（配合 input 标签`name="username"`使用，会有用户名建议）

method：提交数据的 HTTP 方法 （post 或 get）

target：在哪个窗口展示返回数据

事件：onsubmit

#### 五、input 标签：

作用：让用户输入内容

**属性**：

类型 type：决定形式

- text 普通本输入框

- color 选择颜色

- button 按钮

  `<input type="button" value="点击">`

  可使用 button 标签代替

- submit 提交按钮(必须要有，否则提交不了)

  `<input type="submit" value="提交">`

- radio 单选框

  ```
  <input type="radio" name="gender">
  <input type="radio" name="gender">
  ```

- checkbox 复选框

  ```
  input type="checkbox" name="hobby">唱
  <input type="checkbox" name="hobby">跳
  <input type="checkbox" name="hobby">Rap
  ```

- password：密码输入框，输入会被遮挡

- file：文件选择框，multiple 属性，是否允许选择多个文件

  ```
  <input type="file">选择一个文件
  <input type="file" multiple>选择多个文件
  ```

- hidden：不显示在页面

其他属性：

- name：名称

- value：值

**事件**：

onchange/onfocus/onblur(失去)

**验证器：**
HTML5 新增功能。required：是否为必填控件。

#### 六、其他输入标签

- button 标签

  生成一个可以点击的按钮，内部不仅放置文字，还可以放置图像

  type 属性，按钮的类型：submit（提交）、button（不提交）

- textarea 标签：生成多行的文本框

  ```
  <textarea type="resize：none;width: 50%;height 300px ">
  这是一个很长的故事。
  </textarea>
  ```

  `resize：none` 输入框无法拖动

- select+option 标签：生成下拉菜单

  selected 表示默认选择的菜单项

  ```
  <select>
    <option value="0">请选择</option>
    <option value="1" selected>星期一</option>
    <option value="2">星期二</option>
  </select>
  ```

- label：控件的文字说明

#### 注意事项：

- 一般不监听 input 的 click
- form 里面的 input 要有 name
- form 里面要放一个 type=submit 才能触发 submit 事件
