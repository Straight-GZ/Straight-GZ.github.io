---
title: Vue3的一些新特性和一些总结
tags:
  - 框架
  - Vue
---

#### 1、Vue3 和 Vue2 的一些区别

- 创建实例

```
  // Vue3 新方法 createApp 其作用是构建实例
  import { createApp } from 'vue'
  const app = createApp({})
  // Vue2 使用的是 new Vue() 构建实例
```

- Vue3 支持碎片(Fragments)，就是说在组件可以拥有多个根节点

- 增`v-model`代替以前的`v-model`和`.sync`

- 新增`context.emit`，与`this.$event`作用相同
- 具名插槽：
  <!-- more -->

  ```
    <template v-slot:content>
      <strong>hi</strong>
      <div>你好</div>
    </template>
    <template v-slot:title>
      <strong>加粗的标签 </strong>
    </template>


    <header>
        <slot name="title" />  //对应 v-slot:title
        <span @click="close" class="lunzi-dialog-close"></span>
        </header>
    <main>
        <slot name="content" />  //对应 v-slot:content
    </main>
  ```

#### 2、组合式 api

- setup 组件选项

  - 在创建组件之前执行，一旦 props 被解析，就作为组合式 API 的入口。接受两个参数 props 和 context

  - props:响应式的，当传入新的 prop 时，它将被更新。

    不能使用 ES6 解构，因为它会消除 prop 的响应性，需要解构 prop，可以通过使用 setup 函数中的 toRefs

    ```
    import { toRefs } from 'vue'
    setup(props) {
    const { title } = toRefs(props)
        console.log(title.value)
    }
    ```

  * Context:context 是一个普通的 JavaScript 对象，它暴露三个组件的 property

        export default {
         setup(props, context) {
              // Attribute (非响应式对象)
              console.log(context.attrs)
             // 插槽 (非响应式对象)
             console.log(context.slots)
             // 触发事件 (方法)
             console.log(context.emit)
         }
        }
        //可以使用es6解构
        export default {
          setup(props, { attrs, slots, emit }) {
          ...
          }
        }

  * 执行 setup 时，组件实例尚未被创建，将无法访问组件选项：data、computed、methods

* 带 ref 的响应式变量

  接受参数，并将其包裹在一个带有 value property 的对象中返回

  ```
  import { ref } from 'vue'
  const counter = ref(0)
  console.log(counter) // { value: 0 }
  console.log(counter.value) // 读
  counter.value++     //写
  ```

* 在 setup()中 methods、computed、watch、生命周期函数

  ```
  import { computed, ref, onMounted,  watchEffect } from "vue";
  export default {
  props: {
    title: String
  },
  setup (props, context) {
    const counter = ref(0)
    const { user } = toRefs(props)
    const add1=()=>{  // context.emit代替this.$emit
      context.emit('x',counter.value+1
    }
    const add2 = computed(() => {
      return counter.value+2
    })
    onMounted(add1)
    watch(user, add1)   //return 之后才能在模板中使用
    return {counter,add1,add2}
  }
  ```

* Teleport:允许我们控制在 DOM 中哪个父节点下呈现 HTML

  ```
  <teleport to="body">
    <div>
    </div>
  </teleport>  //将 div放在body里面
  ```

#### 3、属性绑定

默认所有属性都绑定到根元素

//**Button.vue**

```vue
<template>
  <div>
    <button><slot /></button>
  </div>
</template>
```

//**ButtonDemo.vue**

```vue
<template>
  <div>button 内容</div>
  <h1>示例</h1>
  <Button @click="" @focus="">按钮</Button> //Button上所有时间属性
  会绑定到Button组件跟元素上
</template>
<script>
import Button from "../lib/Button.vue";
export default {
  components: { Button },
};
</script>
```

- 使用`inheritAttrs:false`可以取消默认绑定
- 使用`$attrs`或者 `context.attrs` 获取所有属性
- 使用 `v-bind='$attrs'`批量绑定属性
- 使用`const {size,onclick,...xxx}=context.attrs`将属性分开

props 和 attrs 的区别

- props 要先声明才能取值，attrs 不需要
- props 不包含事件，attrs 包含
- props 没有声明的属性，会跑到 attrs 里
- props 支持 String 之外的类型，attrs 只支持 String 类型

#### 4、context.slots.default()

**检查组件内部标签类型**

```
setup(props, context) {
  const defaults = context.slots.default();
  defaults.forEach((tag) => {
     if (tag.type !== Tab) {
       throw new Error("Tabs子标签必须是Tab标签);
     }
  })}
```

#### 5、Vue3 支持 TS 和 markdown

支持 TS

```
  //shims.vue.d.ts
  declare module "*.vue" {
  import { ComponentOptions } from "vue";
  const componentOptions: ComponentOptions;
  export default componentOptions;
  }
```

支持 markdown

//shims.vue.d.ts

```
declare module "*.md" {
  const str: string;
  export default str;
}
```

plugin/md.ts

```
// @ts-nocheck
import path from "path";
import fs from "fs";
import marked from "marked";

const mdToJs = (str) => {
  const content = JSON.stringify(marked(str));
  return `export default ${content}`;
};

export function md() {
  return {
    configureServer: [
      // 用于开发
      async ({ app }) => {
        app.use(async (ctx, next) => {
          if (ctx.path.endsWith(".md")) {
            ctx.type = "js";
            const filePath = path.join(process.cwd(), ctx.path);
            ctx.body = mdToJs(fs.readFileSync(filePath).toString());
          } else {
            await next();
          }
        });
      },
    ],
    transforms: [
      {
        // 用于 rollup
        test: (context) => context.path.endsWith(".md"),
        transform: ({ code }) => mdToJs(code),
      },
    ],
  };
}
```
