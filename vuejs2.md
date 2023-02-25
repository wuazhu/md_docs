- Platforms/web/runtime/index.js

````javascript
// 补丁函数(更新函数) 将vdom转化成真实dom
Vue.prototype.__patch__

// 实现$mount方法 转化vdom成真实dom, 挂载到宿主
Vue.prototype.$mount = function() {
  return mountComponent()
}

````



- src/core/index.js

初始化全局api

```javascript
initGlobalAPI(Vue)
```



- src/core/instance/index.js

构造函数

