# Hooks

> Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

## useState(initialValue), 

> useState方法生成一个数组[`state变量`, `state变量更新的方法`]

```javascript
    import React, {useState} from 'react'
    const demo = () => {
        const [count, setCount] = useState(null);
        // 可以在一个组件中多次使用useState
        const [age, setAge] = useState(42);
        const [fruit, setFruit] = useState('banana');
        
        setCount(233)
        console.log(count) // 233
    }   
```

## useEffect(funs, dept)

| props | desc | type |
|--- |--- |--- |
|funs| 在每次render执行完成后执行的方法 | `Function` |
|dept| 执行依赖数组 | `Array` |

> useEffect替代了`componentDidMount`, `componentDidUpdate`和`componentWillUnmount`, 将它们合成为同一个API

`func`参数的return值可以返回一个方法, 用于模拟`componentWillUnmount` 


```javascript
    import React, {useEffect} from 'react'
    const demo = () => {
        const [count, setCount] = useState(null);
        // 跟 useState 一样，你可以在组件中多次使用 useEffect
        useEffect(() => {
            console.log('same like componentDidUpdate')
        })

        useEffect(() => {
            return () => {
                console.log('same with componentWillUnmount')
            }   
        })
    }   
```

> 如果想让`useEffect只执行一次`

```javascript

    import React, {useEffect} from 'react'
    const demo = () => {
        const [count, setCount] = useState(null);
        // 跟 useState 一样，你可以在组件中多次使用 useEffect
        useEffect(() => {
            console.log('only run once, like componentDidMount')
        }, [])
    }   
```


### 总结

> 使用hooks function替代class

```javascript

    import React, {useEffect} from 'react'
    const demo = () => {
        const [count, setCount] = useState(null);

        // 模拟组件加载完成
        useEffect(() => {
            console.log('only run once, like componentDidMount')
            // 初始化数据请求
        }, [])

        // 组件卸载及组件更新
        useEffect(() => {
            console.log('same like componentDidUpdate')
            return function cleanup () {
                console.log('same with componentWillUnmount')
            }   
        })

        // 当count改变时才执行的更新, 监听count
        useEffect(() => {
            console.log('same like componentDidUpdate')
            return function cleanup () {
                console.log('same with componentWillUnmount')
            }   
        }, [count])

        return (
            <div>{count}</div> 
        )
    }  

```