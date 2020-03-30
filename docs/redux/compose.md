# redux/compose

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