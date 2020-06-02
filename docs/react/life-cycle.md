# React ^16.8's lifecycle

## life-cycle

组件挂载的时候

- constructor
- getDerivedStateFromProps
- render
- componentDidMount

组件更新

- getDerivedStateFromProps
- shouldComponentUpdate
- render
- getSnapshotBeforeUpdate
- componentDidUpdate

> `getDerivedStateFromProps(nextProps, preState)`

此方法在组件挂在阶段和组件更新阶段都会在`render`执行之前会被调用一次

让state可以根据新的props参数来对state进行一些操作

返回一个对象来更新 state, 返回null则不跟新state

在render中获取到的state是经过此函数进行处理过后的state

可以尝试下面这个例子

```Jsx

class UserList extends React.Component {
  constructor(props) {
    super(props)
    this.state = {}
  }
  render () {
    console.log(this.state)
    return (......)
  }
}
UserList.getDerivedStateFromProps = (props, state) => {
  const { step = 0 } = state
  console.log({ props, state, step })
  if (!step) {
    return {
      step: step + 1
    }
  } else if (step <= 20) {
    return {
      step: step + 1
    }
  }
  return null
}
```

会发现`render`中打印出的step是在`getDerivedStateFromProps`生命周期中计算后的state

getDerivedStateFromProps主要的应用场景是: 组件的state和props有必然联系的时候, 可以通过这个声明周期对state中的更新(或者说派生(Derived))

> `shoudlComponentUpadate`

根据 shouldComponentUpdate() 的返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。默认行为是 state 每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。

当 props 或 state 发生变化时，shouldComponentUpdate() 会在渲染执行之前被调用, 首次渲染或使用 forceUpdate() 时不会调用该方法

```JSX

    class Demo extends React.Component {
      // ...
      shouldComponentUpdate(nextProps, nextState) {
        if (this.props.some !== nextProps.some) {
          return true
        } else {
          return false
        }
      }
    }

```
