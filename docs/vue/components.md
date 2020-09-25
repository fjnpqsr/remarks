# vue 

## 组件注册

- 全局组件注册

```javascript

    Vue.Component('my-component', {
        template: '...',
        props: '...',
        ...
    })

```

- 局部组件注册

    - .vue文件注册, import导入组件

    ```javascript

        <template>
            <div>this is component template</div>
        </template>

        export default {
            props: [...],
            components: {

            },
            ......
        }

    ```

    - 对象方式注册

    ```javascript
        const MyComponent = {
            template: '',
            props: [...],
            ...
        }

    ```

## 计算属性和侦听属性

## 非prop的属性传递(dom属性传递)

非props属性会自动传递到template最外层的dom节点上

## 自定义事件

## 组件间传值

## vuex

    1. mutations和actions的区别

    mutations 用 Store.commit提交, mutation方法接收的参数是state

    action 用 Store.dispatch分发, action方法接收的参数是与store相同的的context, 所以可以再action内部直接使用context.commit提交mutation,
    
    action还支持异步操作, mutation必须是同步执行, mutation如果是异步执行会导致store的更新变得非常紊乱, 数据错误等

    2. vuex中module的作用