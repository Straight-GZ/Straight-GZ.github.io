---
title: 封装DOM
tags:
  - DOM
  - JavaScript
---

### 封装 DOM 操作，对象风格

#### 一、增加 和 删除

创建节点

```
  create(string) {
    const container = document.createElement("template")
     //template可以包含任意节点
    container.innerHTML = string.trim()
    return container.content.firstChild
  },
```

 <!-- more -->

改变位置

```
//在目标节点前面插入
  before(newNode, node) {
    node.parentNode.insertBefore(newNode, node)
  },

//在目标节点后面插入，也就是在目标节点的下一个节点的前面插入
  after(newNode, node) {
    node.parentNode.insertBefore(newNode, node.nextSibling)
  },
```

作为目标节点子节点

```
  append(parent, node) {
    parent.appendChild(node)
  },
```

作为目标节点父节点

```
  wrap(node, parent) {
    dom.before(parent, node)
    dom.append(parent, node)
  },
```

删除

```
  remove(node) {
    node.parentNode.removeChild(node)
    return node
  },
```

清空

```
  empty(parent) {
    let array = []
    let x = parent.firstChild
    while (x) {
    //删除子节点会使children的长度马上变化，所以不能用for循环遍历
      array.push(dom.remove(x))
      x = parent.firstChild
    }
    return array
  },
```

#### 二、读写操作

读写属性

```
//判断参数数量，执行不同的操作
  attr(node, name, value) {
    if (arguments.length === 3) {
      node.setAttribute(name, value)
    } else if (arguments.length === 2) {
      return node.getAttribute(name)
    }
  },
```

读写文本内容

```
  text(node, string) {
    if (arguments.length === 2) {
      if ("innerText" in node) {    //浏览器兼容
        node.innerText = string
      } else {
        node.textContent = string
      }
    } else if (arguments.length === 1) {
      if ("innerText" in node) {
        return node.innerText
      } else {
        return node.textContent
      }
    }
  },
```

读写 HTML 内容

```
  html(node, string) {
    if (arguments.length === 2) {
      node.innerHTML = string
    } else if (arguments.length === 1) {
      return node.innerHTML
    }
  },
```

读写 style

```
  style(node, name, value) {
    if (arguments.length === 3) {
      node.style[name] = value
    } else if (arguments.length === 2) {
      if (typeof name === "string") {
        return node.style[name]
      } else if (name instanceof Object) {
        const object = name
        for (let key in object) {
          node.style[key] = object[key]
        }
      }
    }
  },
//用法
dom.style(node,"color","red")
dom.style(node,"color")
dom.style(node,{color:"red"})
```

class 增加和删除

```
class: {
    add(node, className) {
      node.classList.add(className)
    },
    remove(node, className) {
      node.classList.remove(className)
    },
    has(node, className) {
      return node.classList.contains(className)
    },
  },
```

添加、移出事件

```
  on(node, eventName, fn) {
    node.addEventListener(eventName, fn)
  },
  off(node, eventName, fn) {
    node.removeEventListener(eventName, fn)
  },
```

#### 三、查找操作

选择器查找

```
 find(selector, scope) {
     //查找范围 全局或给定范围
    return (scope || document).querySelectorAll(selector)
  },
```

获取父元素/子元素/兄弟元素/上一个元素/下一个元素

```
  parent(node) {
    return node.parentNode
  },
  children(node) {
    return node.children
  },
  sibling(node) {
    //获取父元素的子元素排除自己
    return Array.from(node.parentNode.children).filter((n) => n !== node)
  },
  next(node) {
    let x = node.nextSibling
    //如果节点type为3 文本，就找下一个
    while (x && x.nodeType === 3) {
      x = x.nextSibling
    }
    return x
  },
  previous(node) {
    //同上
    let x = node.previousSibling
    while (x && x.nodeType === 3) {
      x = x.previousSibling
    }
    return x
  },
```

遍历

```
  each(nodes, fn) {
    for (let i = 0; i < nodes.length; i++) {
      fn.call(null, nodes[i])
    }
  },
```

获取下标

```
index(node) {
    let list = dom.children(node.parentNode)
    let i
    for (i = 0; i < list.length; i++) {
      if (list[i] === node) {
        break
      }
    }
    return i
  },
```
