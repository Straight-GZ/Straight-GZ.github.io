---
title: 番茄记账React版
tags:
  - 项目
  - React
---

### [预览链接](https://gengzhii.gitee.io/morney-r-website/#/money)

### [源码链接](https://gitee.com/gengzhii/morney-r)

### 一、create-react-app

1. 创建项目
   - 新建项目目录
   - create-react-app . --template typescript (在当前目录创建项目，支持 TS)
   - yarn start
2. CSS normalize [文档](https://create-react-app.dev/docs/adding-css-reset)

   - `@import-normalize; `
   - 作用是让不同浏览器上的默认样式相近
   - css reset 重置默认样式

3. 支持 sass

   [文档](https://create-react-app.dev/docs/adding-typescript#installation)

   - node-sass 下载速度慢，本地编译慢，使用 dart-sass 代替，但 React 不支持 dart-sass
   - `yarn add node-sass@npm:dart-sass`:使用 dart-sass 代替 node-sass [github](https://github.com/facebook/create-react-app/issues/5282)
   - 或者在 package.json 配置，修改 dependencies 配置
     ```
     "node-sass": "npm:dart-sass", "dart-sass": "^1.19.0"
     ```
   - npm6.9 新功能 package alias

 <!-- more -->

4. 绝对引用 [Importing a Component](https://create-react-app.dev/docs/importing-a-component/#absolute-imports)
   - 配置
   ```
   //tsconfig.json
   "compilerOptions": {"baseUrl": "src"},
   "include": ["src"]
   ```
   - 使用：`import Button from 'components/Button';`

### 二、 styled-components

- 安装

  ```
   yarn add styled-components
   yarn add --dev @types/styled-components
  ```

* 使用
  ```
    const Button=styled.button`
        color:red
    `
  ```
* 插件

  webstorm:styled-components
  vscode:vscode-styled-components

### 三、React-Router

- 安装
  ```
    yarn add react-router-dom
    yarn add @types/react-router-dom
  ```

* 引入

  ```
    import {HashRouter as Router, Switch, Route, Redirect} from "react-router-dom";
  ```

* 使用

  - 路由

  ```
    function App() {
        return (
        <Router>
          <Switch>
              <Redirect exact from = "/" to = "/  money"/>  //重定向
              <Route exact path = "/tags">
                  <Tags/>
              </Route>
              <Route exact path = "/tags/:id">
                  <Tag/>
              </Route>
              <Route exact path = "/money">
                  <Money/>
              </Route>
              <Route exact path = "/statistics">
                  <Statistics/>
              </Route>
              <Route exact path = "*">  //除了上面路径之外的404页面
                  <NoMatch/>
              </Route>
          </Switch>
        </Router>
        );
  }
  ```

  - Link 和 传参

    ```
    //tags.tsx
    import {Link} from 'react-router-dom';
    ...
     <Link to = {'/tags/' + tag.id}>    //通过url传递参数
              <span>{tag.name}</span>
              <Icon name = 'right'/>
            </Link>
    ...

    //tag.tsx
    import {useHistory, useParams} from 'react-router-dom';
    ...
    let {id: idString} = useParams<Params>();   //通过useParams拿到
    ...
    ```

  - useHistory

    ```
    ...
    const history = useHistory();
    const onClickBack = () => {history.goBack();};    //goBack 返回上一级
    ...
    ```

    - replace：替换历史堆栈上的当前条目
    - go:按条目移动历史堆栈中
    - goBack:相当于 go(-1)
    - goForward:相当于 go(1)

* NavLink
  ```
   <NavLink to="/" activeClassName="selected"> //被选中会有对应的class
  </NavLink>
  ```

### 四、引入 svg

- `yarn eject`拿到 webpack 配置

* 安装`svg-sprite-loader` `svgo-loader`
* 配置
  ![](https://ftp.bmp.ovh/imgs/2021/03/44d801cafa948ab4.png)

  ```
  {
  	test: /\.svg$/,
  	use: [
  		{loader: 'svg-sprite-loader', options: {}},
  		{
  			loader: 'svgo-loader', options: {
  				plugins: [
  					{removeAttrs: {attrs: 'fill'}},     //删除fill属性(颜色)
  				]
  			}
  		}
  	]
  },
  ```

  **tree shaking** 导入未使用默认会被删除，这就导致 svg 引入之后需要使用才能显示在页面

  ```
    import x from 'icons/chart.svg'
    //console.log(x) 不加这行 icon不显示   或者使用 require('icons/chart')
    <svg>
        <use xlinkHerf='#chart'>
    </svg>
  ```

* 封装 Icon 组件

  ```
    import React from 'react';

    const importAll = (requireContext: __WebpackModuleApi.RequireContext) => requireContext.keys().forEach(requireContext);
    try { importAll(require.context('icons', true, /\.svg$/));} catch (error) { console.log(error);}        //引入整个icons文件夹，避免一个一个引入
    type Props = {
    name?: string
    } & React.SVGAttributes<SVGElement>
    const Icon = (props: Props) => {
    const {name, children, className, ...rest} = props;
    return (
        <svg className = {cs('icon', className)} {...rest}>
        {props.name && <use xlinkHref = {'#' + props.name}/>}
        </svg>
    );
    };
    export {Icon};
  ```

  `__WebpackModuleApi` 报错 `yarn add @types/webpack-env`即解决

* icon 的 onClick

  ```
    type Props = {
    name?: string
    } & React.SVGAttributes<SVGElement>		//类型继承SVG所有属性
    const Icon = (props: Props) => {
    const {name, children, className, ...rest} = props;
    return (
        //将外部传入的className和内部的放在一起
        <svg className = {`icon ${className}`} {...rest}>
        {props.name && <use xlinkHref = {'#' + props.name}/>}
        </svg>
    );
    };
    export {Icon};
  ```

  但是，如果此时外部没有传入 className 属性，当前位置就会变成`undefined`，解决方法是可以用判断语句，或 classNames 库

  ```
  //使用判断
  <svg className = {`icon ${className ? className : ''}`} {...rest}>
    {props.name && <use xlinkHref = {'#' + props.name}/>}
  </svg>
  ```

  ```
  //classnames
    import cs from "classnames";
    ...
  <svg className={cs("icon", className)} {...rest}>
    {props.name && <use xlinkHref={"#" + props.name} />}
  </svg>
  ...
  ```

### 五、自定义 hook

    使用useState等 返回数据读写接口 就是自定义hook

      ```tsx
      import {useState} from 'react';

      const useTags = () => {
      const [tags, setTags] = useState<string[]>(['衣', '食', '住', '行']);
      return {tags, setTags};	//不return数组 避免ts报错
      };
      export {useTags};
      ```

完整代码见 [Gitee](https://gitee.com/gengzhii/morney-r/tree/master/src/hooks)

### 六、受控组件和非受控组件

- 受控模式
  **受控模式** 由 React 监控数据

  ```
  const NoteSection:React.FC = () => {
  const [note,setNote]=useState<string>('')
    return (
      <Wrapper>
        <label>
          <span>备注</span>
          <input type = "text" placeholder = '在这里输入备注'
            value={note}
            onChange={(e)=>setNote(e.target.value)}//调用setNote才会更新数据
          />
        </label>
      </Wrapper>
    );
  };
  ```

* 非受控组件

  ```
    const NoteSection: React.FC = () => {
    const [note, setNote] = useState<string>('');
    const refInput = useRef<htmlinputelement>(null);
    const x = () => {
        if (refInput.current) {
          setNote(refInput.current.value); //通过ref拿到当前数据
        }
        };
      console.log(note);
      return (
            <wrapper>
                <label>
                    <span>备注</span>
                    <input type = "text" placeholder = '在这里输入备注'
                    ref = {refInput}
                    defaultValue = {note}     // 不同于 value
                    onBlur = {x} //如果监听事件，React无法获取数据变化
                    />
                </label>
            </wrapper>
    );
    };
  ```

  [跨平台中文字体解决方案](https://zenozeng.github.io/fonts.css/)
