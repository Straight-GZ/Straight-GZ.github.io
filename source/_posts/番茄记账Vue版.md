---
title: 番茄记账Vue版
tags:
  - 项目
  - Vue
---

#### [预览链接](https://gengzhii.gitee.io/morney-website/#/home)

#### [源码链接](https://gitee.com/gengzhii/morney-v)

#### 一、Vue-CLI 中使用引入 SVG

##### 1、使用 svg-sprite-loader

安装：
[官方文档](https://github.com/JetBrains/svg-sprite-loader)

```
npm install svg-sprite-loader -D

yarn add svg-sprite-loader -D
```

在官方文档中有教我們如何在 webpack 配置，在 vue-cli 中就稍微有些麻烦，见[Vue CLI 文档](https://cli.vuejs.org/zh/guide/webpack.html#%E4%BF%AE%E6%94%B9-loader-%E9%80%89%E9%A1%B9)
配置文件:

   <!-- more -->

```
//vue.config.js
const path = require('path')
module.exports = {
  chainWebpack: config => {
      const dir = path.resolve(__dirname, 'src/assets/icons')
      config.module
          .rule('svg-sprite')
          .test(/\.svg$/)   //.svg结尾的
          .include.add(dir).end() //只包含icons目录
          .use('svg-sprite-loader').loader('svg-sprite-loader').options({extract: false}).end()
          .use('svgo-loader').loader('svgo-loader')
          .tap(options => ({...options, plugins: [{removeAttrs: {attrs: 'fill'}}]})).end()
      config.plugin('svg-sprite').use(require('svg-sprite-loader/plugin'), [{plainSprite: true}])
      config.module.rule('svg').exclude.add(dir) //其他 svg loader 排除 icons目录
  }
}
```

配置完成之后，就可以以 symbol 的形式引入 svg，然后就可以使用 use 标签使用

##### 2、Icon 组件

svg 文件一个一个引入太麻烦，可以封装一个 Icon 组件一次性引入 Icon，然后全局注册，这样就可以很方便的在需要的地方使用。

但是，Icon 组件中仍然需要将 SVG 一个一个的引入进来，能不能一次性引入一个目录呢？

答案是可以的。

```
const importAll = (requireContext: __WebpackModuleApi.RequireContext) => requireContext.keys().forEach(requireContext);
importAll(require.context('../assets/icons', true, /\.svg$/));
```

这样，在此目录下的所有.svg 文件就被一次性引入了。
下面是封装的 icon 组件

```
<template>
  <svg class="icon">
    <use :xlink:href="'#'+name"/>
  </svg>
</template>

<script lang="ts">
const importAll = (requireContext: __WebpackModuleApi.RequireContext) => requireContext.keys().forEach(requireContext);
try {	//防止报错
  importAll(require.context('../assets/icons', true, /\.svg$/));
} catch (error) {
  console.log(error);
}
export default {
  name: "Icon",
  props: ['name']
};
</script>
```

如此，使用 icon 只需要将 icon 的 id 作为 Icon 组件的外部属性 name 传入，之后再增加新的 icon 只需要将文件放入 icons 目录，就可以直接使用。

#### 二、Vue 组件三种方式（单文件组件）

1. 用 JS 对象

   `export default {data,props,methods,created,...}`

2. 用 TS 类

   ```js
   export default class 组件名 extends Vue {
     xxx: string = "hi";
     @prop(Number) xxx: number | undefined;
   }
   ```

3. 用 JS 类 (和 TS 类似)

   `export default class 组件名 extends Vue{xxx='hi'}`

4. vue-property-decorator

   更好的支持 TS

   ```
     import Vue from 'vue';
     import {Component, Prop} from 'vue-property-decorator';
     @Component({
       components: {Button, FormItem},
     })
     export default class FormItem extends Vue {
       @Prop({default: ''}) readonly value!: string; //prop
       @Prop() options?: EChartsOption;
       ...
       get tags() {    //计算属性 computed
         return this.$store.state.tagList;
       }
       ...
       @Watch('options')   //watch
       onOptionsChange(newValue: EChartsOption) {
         this.chart && this.chart.setOption(newValue);
       }
     }
   ```

#### 三、搜集组件的数据

子组件触发事件,将数据发送到父组件

```js
this.$emit("update:value", this.selectedTags);
```

默认数据可以是外部传入的 Prop，这样可以使用.sync 语法

```xml
<Tags :data-source.sync="tags" :value.sync="record.tags"/>
```

#### 四、数据迁移

```js
//获取当前版本号
const version = window.localStorage.getItem("version");
//获取当前数据
const recordList: Record[] = JSON.parse(
  window.localStorage.getItem(recordList)
);
if ("version" === "0.0.1") {
  recordList.forEach((record) => {
    //遍历数据
    record.createTime = new Date(2020, 0, 1); //将每个数据增加时间
  });
  //保存数据
  window.localStorage.setItem("recordList", JSON.stringfy(recordList));
} else if ("version" === "0.0.2") {
  //迁移
}
//设置当前版本号
window.localStorage.setItem("version", "0.0.3");
```

迁移数据只需要将版本一个版本一个版本的向上迁移

#### 五、Vuex

```
import Vue from 'vue';
import Vuex from 'vuex';
import clone from '@/lib/clone';
import createId from '@/lib/createId';

Vue.use(Vuex);

const store = new Vuex.Store({
//存储数据  使用this.$store.state.recordList获取数据
  state: {
    recordList: [] as RecordItem[],
    createTagMessage: '',
    tagList: [] as Tag[],
    currentTag: undefined,
  } as RootStore,
//对数据的读写  没有返回值  使用this.$store.commit('fetchTags',参数)
  mutations: {
    setCurrentTag(state, id: string) {
      state.currentTag = state.tagList.filter(d => d.id === id)[0];
    },
    fetchTags(state) {},
    createTag(state, name: string) {
    },
    removeTag(state, id: string) {},
    saveTags(state) {},
    updateTag(state, object: { id: string; name: string }) {},
    fetchRecords(state) {},
    createRecord(state, record: RecordItem) {},
    saveRecord(state) {
      window.localStorage.setItem('recordList', JSON.stringify(state.recordList));
    }
  },
});
export default store;
```

#### 六、使用多个`:class`

```
<li :class="value==='-' && 'selected'}"  //类名加引号 @click="selectType('-')">支出</li>

<li :class="{[classPrefix+'-item']:classPrefix,'selected':value==='-'}" @click="selectType('-')">支出</li>

{
//如果存在classPrefix 就存在前面的类名  如果类名存在变量，使用[]
[classPrefix+'-item']:classPrefix,
'selected':value==='-'
}
```
