[toc]

### setState

setState 在合成事件和生命周期中是异步批量更新的, 达到优化目的

setState 在 setTimeout 和原生事件(通过 document.addEventListener), 在 setState 的回调事件中是同步的

setState 的更新可能会被合并更新

### redux

可以通过 store.getState 来获取全局 state 状态

添加 react-redux 可以通过 Provider 来把数据全局灌入, 通过 connect 组件把数据mapStateToProps



### React-Router

Route 来承载匹配组件, 通过 Route组件的 children>component>render来渲染组件, 三者互斥, 只能使用一种

```react
<Link to="/">首页</Link>
<Link to="/user">用户</Link>
<BroswerRouter>
	<Switch>
  	<Route path="/" component={IndexPage}></Route>
    <Route path="/user" children={()=>IndexPage}></Route>
    <Route render={()=> <div>404page</div>}></Route>
  </Switch>
</BroswerRouter>

```



### PureComponent

1. 浅比较, 设计对象嵌套就无法优化渲染
2. 本质是对 shouldComponentUpdate 的 nextProp, nextState 进行比较优化, 如果相等就不更新返回 false, 

