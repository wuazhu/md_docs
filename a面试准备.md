## 事件循环 eventloop

> 1. JavaScript是单线程, 分执行栈(主线程)跟任务队列, 所有的同步操作都会在执行栈里
> 2. 主线程外还有一个任务队列, 只要异步任务有了运行结果就在任务队列里放置一个事件
> 3. 一旦执行栈同步任务执行完毕, 就会读取 **任务队列**, 看看有哪些事件
> 4. 重复步骤3

- 宏任务

  setTimeout

  setInterval

  xhr

  I/O操作

  UI交互

  requestAnimationFrame

  setImmediate(nodejs)

- 微任务

  Promise.then 回调

  mutationObserver

- Process.nextTick/setImmediate
  
  Process.nextTick 可以在当前执行栈(同步)的尾部下一次的 EventLoop之前触发回调, 也就是说他指定的任务总是发生在所有异步任务之前
  
  setImmediate 在当前任务队列的尾部添加事件, 总是在下一次EventLoop时执行

什么是事件队列

js是单线程, 主线程执行同步代码, 会有一个执行栈, 主线程外还有一个任务, 遇到一些异步操作比如settimeout ajax请求等事件有结果了 就会往任务队列放一个事件, 执行栈执行完所有的同步操作了,就会读取任务队列, 看看有哪些事件进入执行栈

```js
console.log('1');
setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')；
    })；
})；
process.nextTick(function() {
    console.log('6');
})；
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')；
})；
 
setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })；
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')；
    })；
})；
// 第一轮解析 输出1 7
log(1)
setTimeout异步宏任务,标记为 **timeout1**
process.nextTick异步微任务,标记为 **process1**
new Promise 里面的 log7直接打印 后面的.then 微任务标记为 **then1**
setTimeout异步宏任务,标记为 **timeout2**

//当前的宏任务: timeout1 timeout2
//当前的微任务: process1 then1

  
// 第二轮解析 输出 6 8
// 先执行前面的微任务 process1 then1

  


```



## webpack

webpack插件怎么写

webpack plugin跟loader区别

webpack 优化

## Ts

- type与interface区别



## vue

vue

路由模式 hash history

#### 数据响应式

通过Object.defineProperty中设置get 来获取值跟set来追踪绑定需要响应的dep, 来通知dep对应的更新函数

- vue1

  每一个key都对应一个dep, 在编译的时候每一个响应式对象都产生一个watcher, 在初始化的时候会读取一次响应式对象的getter, 触发dep与watcher的绑定, 在setter里去调用dep.notify() 触发对应的watchers更新

​		缺点: 每一个动态节点都有一个watcher与之对应,

- vue2

- watcher 跟dep

- defineProperty与proxy的区别 

- defineProperty可以设置什么

- vuex piana

  [vue2](./vuejs2.md)

- new Vue都做了什么

```javascript
initLifecycle(vm) // 生命周期相关的一些属性初始化, $parent,$root,$children等等
initEvents(vm) // 自定义组件事件相关监听
initRender(vm) // 初始化render相关, 插槽处理, $createEle == render(h)
callHook(vm, 'beforeCreate') // 触发beforeCreate 生命周期
// 组建数据和状态的初始化start
initInjections(vm) // resolve injections before data/props 隔代传参的数据, 祖辈数据传入后才有后续的Provide
initState(vm) // data/props/watch/methods/computed等等响应式处理过程
initProvide(vm) // resolve provide after data/props
// 组建数据和状态的初始化end
callHook(vm, 'created') // 调用created生命周期
```

组建执行$mont的时候会生成1更新函数, 2watcher, 一个组建一个watcher



## http



## 小程序

- 最近更新的语法
- 支付流程
- 优化, 白屏处理
- 

## 原生JS基础

- 闭包

  > 函数访问了父级及父级以上的作用域变量, 那么这个函数就是一个闭包
  >
  > ```js
  > var a = 0
  > (function aa() {
  > 	alert(a)
  > })()
  > 
  > function a() {
  > 	var i = '1'
  >   i = i+'b'
  >   function c() {
  >     i = i+'c'
  >     console.log(i)
  >   }
  >   return b
  > }
  > var c = a() // 1b
  > c()// 1bc
  > c() // 1bcc
  > c() // 1bccc
  > ```

  优点:

  1. 访问函数内部变量
  2. 让变量始终保持内存

  缺点:

  1. 内存泄漏, 一只存在内存中消耗大, 性能问题, 解决办法:使用完置为null
  2. 可能在父函数外部, 改变内部变量的值
  3. 性能损耗



Q:缓存 协商缓存/强缓存 http缓存

Q: webpack的优化有哪些

Q: gzip对应 

Q: 数组队列操作 环形队列 https://juejin.cn/post/7038216569385680910

Q: XFS

> 说明: 用户通过iframe装载被攻击网站, 劫持记录用户操作
>
> 解决方法: X-FRAME-OPTIONS标头
>
> ①DENY -----表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
>
> ②SAMEORIGIN -----表示该页面可以在相同域名页面的frame中展示；
>
> ③ALLOW-FROM uri -----表示该页面可以在指定来源的 frame 中展示

Q: XSS

> 说明: 通过html标签插入script等等
>
> 通过正则匹配传入字符串替换html编码	

Q: CSRF

> 说明: csrf是通过伪装成站点用户进行攻击，而且防范的资源也少，难以防范.
>
> ```undefined
> 例如：你登录网站，并在本地种下了cookie如果在没退出该网站的时候 不小心访问了恶意网站，而且这个网站需要你发一些请求等此时，你是携带cookie进行访问的，那么你的重在cookie里的信息就会被恶意网站捕捉到，那么你的信息就被盗用，导致一些不法分子做一些事情
> ```
>
> 解决方法:
>
> 1.请求头中有referer字段来验证来源
>
> 2. 请求中添加token并验证
> 3. 在http头中自定义属性并验证

- promise async await promise原理
- 节流防抖
- vue组建通信
- node中间件
- 事件队列, 微任务宏任务
- 原型 原型链 继承
