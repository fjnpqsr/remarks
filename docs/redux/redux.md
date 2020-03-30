# redux

### Redux 是 JavaScript 状态容器，提供可预测化的状态管理。

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

> action

action 是把数据从应用传到store的载体, 它是store数据的唯一来源

> reducer

reducer 制定了应用状态的变化如何响应action并发送到store

reducer 接收旧的state和action, 返回新的state

(previousState, action) => newState

使用combineReducer组合多个reducer

> store

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


> createStore(reducer, [placeholderState], enhancer)

