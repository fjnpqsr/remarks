
# react-redux

> connect(mapStateToProps)

从 Redux state 树中读取部分数据，并通过 props 来把这些数据提供给要渲染的组件

mapStateToProps 这个函数来指定如何把当前 Redux store state 映射到展示组件的 props 中




```javascript

//  将store注入props的函数
const mapStateToProp = state => ({
    ...state
})

// 注入组件的回调函数
const mapDispatchToProps  = (dispatch, ownProps) => ({
    ...
})

// 使用connect讲组件和store绑定起来
export default connect(mapStateToProp, mapDispatchToProps)(Component)


```

- `ownProps`: 组件创建时传入到组件的属性, 不包括从store中获取的数据

```javascript

class Exception extends React.Component {
  constructor (props) {
    super(props)
    this.state = {age: 1}
  }
  render () {
    console.log(this.props) // here includes products: undefined
    return (<h1 >403</h1>)
  }
}

// pass products: undefined to components
const mapStateToProps = (state) => ({ products: state.products }) 

const mapDispatchToProps = (dispatch, ownProps) => {
  console.log({ ownProps }) // not includes products: undefined
  ...
}

export default connect(mapStateToProps, mapDispatchToProps)(Exception)
```

