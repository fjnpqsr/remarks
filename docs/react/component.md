# React.Component 和 React.PureComponent的区别

React.PureComponent 与 React.Component 很相似。两者的区别在于 ```React.Component 并未实现 shouldComponentUpdate()```，而 React.PureComponent 中以浅层对比 prop 和 state 的方式来实现了该函数。

仅在你的 props 和 state 较为简单时，才使用 React.PureComponent, 或者在深层数据结构发生变化时调用 forceUpdate() 来确保组件被正确地更新。

例如: 在PureComponent中, 通过`this.setState({arr: this.state.arr.push('newData')})`,

在setState()的过程中, this.state.arr的内存地址未发生改变, 更新前后this.state.arr始终指向同一个数组, 所以页面不会对此操作进行重新渲染.

可以通过`this.setState({arr: this.state.arr.push('newData').slice(0)})`, 或者其他能够生成新的数组的方法去让state更新前后的arr的内存指针发生变化, 触发页面的更新

此外，React.PureComponent 中的 shouldComponentUpdate() 将跳过所有子组件树的 prop 更新。因此，请确保所有子组件也都是“纯”的组件。
