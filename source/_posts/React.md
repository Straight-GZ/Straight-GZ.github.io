---
title: React 基本用法
tags:
  - React
  - 框架
---

### 一、createElement 和 JSX

1. createElement

创建并返回指定类型的新 React 元素。其中的类型参数既可以是标签名字符串（如 'div' 或 'span'），也可以是 React 组件 类型 （class 组件或函数组件）,代码编写复杂

```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

2. JSX
   下面代码会被编译为和上面相同的代码，完全等价

   ```
   const element = (
   <h1 className="greeting">
       Hello, world!
   </h1>
   );
   ```

   注意：

   - 在 JSX 中给元素添加类, 需要使用 className 代替 class
   - JSX 中可以直接使用 {} 中间写 JS 代码
   <!-- more -->

### 二、class 组件和生命周期钩子

1、 class 组件

```
class Demo extends React.Component {
constructor(props) {
    super(props);   //必须用super调用父类的constructor
    this.state = {n: 1};
}
add(){this.setSTate((state)=>    //使用函数
     return {n:this.state.n+1})}
render() {
    return (
    <div>
        {this.state.n}
        <button onClick={()=>this.add()}>+1</button>
    </div>
    );
}
}
```

2、 props 和 state

- props

  - 给组件传递数据，一般用在父子组件之间
  - React 把传递给组件的属性转化为一个对象并交给 props
  - props 是只读的，无法给 props 添加或修改属性
  - props.children：获取组件的内容

- state
  - 用来给组件提供组件内部使用的数据
  - 只有通过 class 创建的组件才有，函数组件使用 useState
  - setState 是异步的，改变之后马上取取不到最新的值
  - setState 会自动合并第一层属性，useState 不会

3、生命周期钩子

1. constructor

   - 初始化 props、state
   - 此时不能调用 setState
   - bind this

   ```
   constructor(props) {     //如果只是初始化props 可以省略
       super(props);
       this.state = {n: 1};
       this.onClick=this.onClick.bind(this)
   }
   //等价于 写在 constructor外面
   onClick=()=>{}
   ```

2.shouldComponentUpdate

- 返回 false 阻止 UI 更新
- 返回 true 不阻止
- 作用：允许手动判断是否需要更新组件，避免不必要的更新
- 如果返回值为 false，那么，后续 render()方法不会被调用
- 可以使用 React.PureComponent 代替 React.Component,两者的区别在于 React.Component 并未实现 shouldComponentUpdate()，而 React.PureComponent 中以浅层对比 prop 和 state 的方式来实现了该函数。[官方文档](https://zh-hans.reactjs.org/docs/react-api.html#reactcomponent)

3. render

- 展示视图
- 只能有一个根元素
- 多个根元素可以使用<React.Fragment>简写为 <></>
- 函数能够执行多次，只要组件的属性或状态改变了，这个方法就会重新执行

4. componentDidMount

- 组件已经挂载到页面中
- 可以进行 DOM 操作，比如获取 DOM 元素的宽高属性
- 可以**发送请求**获取数据（**官方推荐**）

5. componentDidUpdate

- 组件已经更新
- 不能修改状态 否则会循环渲染（除非条件判断）
- 首次渲染不会执行此方法
- shouldComponentUpdate() 返回值为 false，则不会调用 componentDidUpdate()。
- 参数：旧的属性和状态对象

  ```
  componentDidUpdate(prevProps, prevState, snapshot)
  ```

6. componentWillUnmount()

- 组件卸载,组件一辈子只能执行一次
- 执行清理工作，清除定时器等

### 三、函数组件和 hooks

1、函数组件

    ```
    function Demo(props) {
    const [n,setN]=React.useState(0)
    return <h1>Hello, {props.name}</h1>;
    }
    ```

2、hooks

- useState

  - 使用状态
  - 不可局部更新，如果 state 是一个对象，并不会合并属性，可以使用...操作符
  - 对象要改变地址，React 才认为数据改变了
  - 初始化和 set 的时候可以使用函数

  ```
  const [n,setN]=React.useState(()=>{return initialState}))

  setN(x=>x+1)
  ```

* useReducer

  useState 替代方案

  ```
  const initialState = {count: 0};      //创建初始值
  function reducer(state, action) {     //创建所有操作
    switch (action.type) {
        case 'increment':
        return {count: state.count + 1};
        case 'decrement':
        return {count: state.count - 1};
        default:
        throw new Error();
    }
  }
   function Counter() {
    const [state, dispatch] = useReducer(reducer, initialState);    //传给useReducer，获取读写操作
    return (
        <>
        Count: {state.count}
         //调用对应操作
        <button onClick={() => dispatch({type: 'decrement'})}>-</button>
        <button onClick={() => dispatch({type: 'increment'})}>+</button>
        </>
    );
    }
  ```

* useContext

  ```
  const X=React.createContext(null)     //创建上下文
  function App(){
      const [n,setN]=useState(0)
      return (
          <X.Provider value={{n,setN}}> //划定区域 绑定数据
            <Demo1/>
          </X.Provider>
      )
  }
  function Demo1(){
      return (
          <Demo2/>
      )
  }
  function Demo2(){
      const {n,setN}=useContext(X)     //拿到数据
      return (
          <div>
            {n}
            <button onClick={()=>setN(x=>x+1)}>+1</>
          </div>
      )
  }
  ```

**使用 useReducer 和 useContext 代替 Redux**
[代码](https://codesandbox.io/s/interesting-volhard-lfxpm)

- useEffect

  在函数组件中执行副作用操作

  ```
    useEffect(()=>{},[])    //只在第一次渲染执行 相当于componentDidMount
    useEffect(()=>{},[x])    //在数组中依赖变化时执行
    useEffect(()=>{})
    useEffect(()=>{
        return ()=>{}  // 在组件卸载时执行，清除计时器之类的操作
    })      //每次渲染都执行
  ```

  useLayoutEffect:它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，useLayoutEffect 内部的更新计划将被同步刷新

* useMemo、React.memo、useCallback

  1. React.memo：组件在相同 props 的情况下渲染相同的结果，那么你可以通过将其包装在 React.memo 中调用,props 不变就不会重新渲染，**如果 prop 是一个复杂对象，可以使用 useMemo**[文档](https://zh-hans.reactjs.org/docs/react-api.html#reactmemo)
  2. 把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。类似 Vue 的 computed
  3. useCallback(fn, deps) 相当于 useMemo(() => fn, deps)。

* useRef

  - 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内保持不变。
  - React.forwardRef 会创建一个 React 组件，这个组件能够将其接受的 ref 属性转发到其组件树下的另一个组件中。**让函数组件在接受 props 之外，再接受一个 ref**[文档](https://zh-hans.reactjs.org/docs/react-api.html#reactforwardref)
  - useImperativeHandle 可以让你在使用 ref 时自定义暴露给父组件的实例值[文档](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)

* 自定义 hook

  自定义 Hook 是一个函数，其名称以 “use” 开头，函数内部可以调用其他的 Hook。[例子](https://codesandbox.io/s/recursing-darkness-zuo4b)
