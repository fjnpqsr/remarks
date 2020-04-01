# applyMiddleware

根据官网的描述

> 使用包含自定义功能的 middleware 来扩展 Redux 是一种推荐的方式。Middleware 可以让你包装 store 的 dispatch 方法来达到你想要的目的。

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

## 总结

`enhancer, applyMiddlewares`是用来对createStore进行封装的函数, applyMiddlewares内部先实例化store,通过`compose()`函数对`middlewares`中间件进行合并, 组合成一个最终的`dispatch`函数替代store实例的dispatch
