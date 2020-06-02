# redux

> ## what is redux ?

Redux 是 JavaScript 状态容器，提供可预测化的状态管理。

redux 主要用于处理的数据

- 服务器响应
- 缓存数据
- 本地生成尚未持久化到服务器的数据
- UI状态等等

Redux 试图让 state 的变化变得可预测

### 核心概念

1. 强制使用action来描述所有变化, 可以清晰的知道应用中到底发生了什么
2. 通过reducer把action和state串联起来

### 三大原则

1. 单一数据源: 整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中
2. state是只读的: 唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象
3. 使用纯函数来执行修改: 为了描述 action 如何改变 state tree ，你需要编写 reducers


### 基础

#### action

action 是把数据从应用传到store的载体, 它是store数据的唯一来源

#### reducer

reducer 制定了应用状态的变化如何响应action并发送到store

reducer 接收旧的state和action, 返回新的state

(previousState, action) => newState

使用combineReducer组合多个reducer

#### store

redux应用下只能有一个单一的store

当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

- getState() 获取state
- dispatch(action) 跟新state (函数执行后返回action)
- 通过 subscribe(listener) 注册监听器;
- 通过 subscribe(listener) 返回的函数注销监听器。

```javascript
    // 创建 store
    import {createStore} from 'redux'
    import { todoApp } from './reducers'
    let store = createStore(todoApp)
```

### api

- createStore(reducer, [placeholderState], enhancer)

> ## compose

compose: 组成，构成(一个整体)

意味着将多个东西组合成一个整体

例如我们要组合`['h', 'e', 'l', 'l', 'o', ',' ,'w', 'o', 'r', 'l', 'd']`成为字符串`hello,world`

- 通过join('')去组合

但是如果场景为每项都是一个对象, exp: `[{text: 'h'}, {text: 'e'}, {text: 'l'}, {text: 'l'}, {text: '0'}]`

- 通过初始化变量, 用forEach函数去遍历

```javascript
  let result = ''
  const data = ['h', 'e', 'l', 'l', 'o', ',' ,'w', 'o', 'r', 'l', 'd']
  data.forEach(function (item, index) {
    result = result + item
  })
  console.log(result)
  // 输出 hello, world
```

但是如果场景为每项都是一个函数, exp: `[() => 'h', () => 'e', () => 'l', () => 'l', () =>'o']`

```javascript
  // 封装及增加初始值初始值
  const compose = function (data, initivalValue) {
    console.log({data, initivalValue})
    data.forEach(function (item, index) {
      initivalValue = initivalValue + item()
    })
    return initivalValue
  }
  console.log(compose([() => 'h', () => 'e', () => 'l', () => 'l', () =>'o'], '*'))
```

如果我们要实现把每个函数的结果传递给下一个函数, 实现函数的连续调用

```javascript


  // 我们定义几个操作函数, 加减陈处
  const add2 = (num) =>  num += 2
  const decount2 = (num) =>  num -=2
  const multiply2 = (num) =>  num *= 4
  const devide2 = (num) =>  num /= 2
  // 我们相对一个数字进行加减陈除的调用

  const chainOrders = function (num) {
    let afterAdd = add2(num)
    let afterDecount = decount2(afterAdd)
    let afterMultiply = multiply2(afterDecount)
    let afterDevide = devide2(afterMultiply)
    return afterDevide
  }
  
  console.log(chainOrders(2))
```

在`chainOrder`中, 下一个函数的参数总是使用上一个函数的返回值

这就和Array.reduce类似

尝试通过Array.reduce()来实现

``` javascript
  const add2 = (num) =>  num += 2
  const decount2 = (num) =>  num -=2
  const multiply2 = (num) =>  num *= 2
  const devide2 = (num) =>  num /= 2
  
  const funs = [add2, decount2, multiply2, devide2]
  
  const baseNum = 1
  funs.reduce((lastestValue, current) => {
    console.log({lastestValue, current: current.name})
    // {latestValue: 1, current:'add2'}
    // {latestValue: 3, current:'decount2'}
    // {latestValue: 1, current:'multiply2'}
    // {latestValue: 2, current:'devide2'}
    return current(lastestValue)
  }, baseNum)

  //设计我们的调用方式
  //  const result = compose(funcs)(initialValue)
  args和initialValue分开传递, 方便在compose中直接通过args获取到函数数据




  // 将baseNum封装进conpose



  // const compose = function (...args) {
  //   console.log({args})
  // }
  // compose(add2, decount2, multiply2, devide2) // console.log(Array(4))
  // compose([add2, decount2, multiply2, devide2]) // console.log(Array(1)) Array[0] = Array[4], 这种调用方式太麻烦, 数据结构层级多了一层

  // 我们就使用   compose(add2, decount2, multiply2, devide2) 这是方式

  const add2 = (num) =>  num += 2
  const decount2 = (num) =>  num -=2
  const multiply2 = (num) =>  num *= 2
  const devide2 = (num) =>  num /= 2

  const compose = function(...funs) {
    return funs.reduce((latestValue, current) => (...args) => current(latestValue(...args)))
  }
  // function compose(...funcs) {
  //   return funcs.reduce((a, b) => (...args) => a(b(...args)))
  // }
  const baseNum = 2
  
  compose(add2, decount2, multiply2, devide2)(baseNum)

```

> ## applyMiddleware

根据官网的描述

使用包含自定义功能的 middleware 来扩展 Redux 是一种推荐的方式。Middleware 可以让你包装 store 的 dispatch 方法来达到你想要的目的。

`applyMiddleware`其实是对`store.dispatch`函数的扩展

最常见的应用是像`redux-thunk`, `redux-Promise`, `redux-saga`对`store.dispatch`进行扩展,实现`异步action`

从代码上看

```javascript
import { createStore, applyMiddleware } from 'redux'

let store = createStore(
  todos,
  [ 'Use Redux' ],
  applyMiddleware(logger)
)

// something like

let store = createStore(
  todos,
  applyMiddleware(logger)
)

```

  ```javascript
  export default function createStore(reducer, preloadedState, enhancer) {

    if (typeof preloadedState === 'function' && typeof enhancer === 'undefined') {
      enhancer = preloadedState
      preloadedState = undefined
    }

    if (typeof enhancer !== 'undefined') {
      if (typeof enhancer !== 'function') {
        throw new Error('Expected the enhancer to be a function.')
      }

      return enhancer(createStore)(reducer, preloadedState)
    }
  }
```

redux的源码中对第二个参数`preloadedState`进行了判断, 当`enhancer`存在的时候, 执行`enhancer(createStore)(reducer, preloadedState)`语句, 而`enhancer`其实就是`applyMiddleware`

那我们再看一下`applyMiddleware`是如何定义的

```javascript
  export default function applyMiddleware(...middlewares) {
    return (createStore) => (reducer, preloadedState, enhancer) => {
      const store = createStore(reducer, preloadedState, enhancer)
      let dispatch = store.dispatch
      let chain = []

      const middlewareAPI = {
        getState: store.getState,
        dispatch: (action) => dispatch(action)
      }
      chain = middlewares.map(middleware => middleware(middlewareAPI))
      dispatch = compose(...chain)(store.dispatch)

      return {
        ...store,
        dispatch
      }
    }
  }
```

applyMiddleware(或者说enhancer)接收`middlewares`, 返回一个`(createStore) => {}`函数
结合`createStore`函数的返回值`enhancer(createStore)(reducer, preloadedState)`来看

```javascript
  export default function createStore(reducer, preloadedState, enhancer) {

    if (typeof preloadedState === 'function' && typeof enhancer === 'undefined') {
      enhancer = preloadedState
      preloadedState = undefined
    }

    if (typeof enhancer !== 'undefined') {
      if (typeof enhancer !== 'function') {
        throw new Error('Expected the enhancer to be a function.')
      }

      return function () {
        const store = createStore(reducer, preloadedState)
        let dispatch = store.dispatch
        let chain = []

        const middlewareAPI = {
          getState: store.getState,
          dispatch: (action) => dispatch(action)
        }
        chain = middlewares.map(middleware => middleware(middlewareAPI))
        dispatch = compose(...chain)(store.dispatch)
        return {
          ...store,
          dispatch
        }
      }
    }
  }
```

`applyMiddleware/enhancer`其实是对`createStor`的一种包装, 通过实例化store, 对实例化的store进行扩展和继承, 所有的middleware都主要是对dispatch的二次封装,也可以清晰的看出`middlewares`的参数来自这个实例化后的store的`getState`和`dispatch`

middlewares的主要结构为

```javascript

function ({getState, dispatch}) {}

```

我们再来看一下这一行代码

`dispatch = compose(...chain)(store.dispatch)`

`compose(...chain)`接受了一个实参`store.dispatch`, 并且最终需要返回一个dispatch(action)的函数, dispatch为接受一个action对象 返回dispatch(action)的结果的函数

所以`compose(...chain)` 目的在于返回如下

```javascript
  function (dispatch) {
    return action => {
      const result = dispatch(action)
      return result
    }
  }
```

结合middlewares的主要结构来看

一个middlewares的结构如下

```javascript
function ({getState, dispatch}) {
  return function (dispatch: next) { // dispatch命名空间重复, redux采用next
    return action => {
      const result = dispatch(action)
      return result
    }
  }
}

```

### 总结

`enhancer, applyMiddlewares`是用来对createStore进行封装的函数, applyMiddlewares内部先实例化store,通过`compose()`函数对`middlewares`中间件进行合并, 组合成一个最终的`dispatch`函数替代store实例的dispatch
