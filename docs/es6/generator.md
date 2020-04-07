# generator

> Generator 函数是 ES6 提供的一种异步编程解决方案

执行 Generator 函数会返回一个[遍历器对象](https://es6.ruanyifeng.com/#docs/iterator), 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象(遍历器对象),可以通过`next()`进行调用, 必须调用遍历器对象的next方法，使得指针移向下一个状态。

```javascript

function* helloWorldGenerator() {
  console.log('will output hello')
  yield 'hello';
  console.log('will output world')
  yield 'world';
  console.log('will output ending')
  return 'ending';
}

var hw = helloWorldGenerator();

```

每一次调用`hw.next()`方法, `generator`函数都会执行值下一个`yield`出现的地方, 就会返回一个有着value和done两个属性的对象。value属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；done属性是一个布尔值，表示是否遍历结束。
