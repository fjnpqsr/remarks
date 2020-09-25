# 生命周期

- beforeCreate

- created

- beforeMount

- mounted

- beforeUpdate

- updated

- beforeDestroy 

    解除绑定, 销毁子组件以及取消时间监听器

- destroyed


1. created和mounted的区别

    created是在视图渲染之前调用,
    mounted则是在模板挂载后调用,

    获取dom节点应该在mounted之后去调用,
    不影响dom的操作可以再created中调用

2. vue应该在哪个声明周期中做数据初始化的请求

- beforeCreate

- created

- beforeMount

- mounted

能够做初试化数据请求的大致有这四种情况

    1. beforeCreate 生命周期中, 实例并未创建完成, 无法访问methods/data等属性创建的方法或者对象(为挂载到实例上), 不考虑
    2. created声明周期中, 可以获取实例信息
    3. beforeMount声明周期中, 可以获取实例信息
    4. mounted 可以获取实例信息, 可以获取组件模板的dom节点

如果不需要获取dom节点信息可以再created中提升初始化数据的请求

需要获取dom节点的操作可以再mounted中实现

