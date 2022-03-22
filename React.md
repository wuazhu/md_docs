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

1. 浅比较, 涉及对象嵌套就无法优化渲染
2. 本质是对 shouldComponentUpdate 的 nextProp, nextState 进行比较优化, 如果相等就不更新返回 false, 



### 生命周期

static defaultProps

constructor

componentWillMount

render

componentDidMount

👇🏻**组件运行时**👇🏻

componentWillReceiveProps(nextProps) {}

> 第一次挂载不执行, 只有已挂载的组件,如果 props 改变将会进入这个生命周期, 然后再进入 shouldComponentUpdate 生命周期

shouldComponentUpdate(nextProps, nextState) {return true/false}

> 如果 state 改变将会进入这个生命周期

componentWillUpdate

render

componentDidUpdate

componentWillUnmount

**新增生命周期**

static getDerivedStateFromProps(nextProps, prevState) {return null}

> 会在 render 之前调用, 如果之前有 shouldComponentUpdate 会在他之前执行, 在初始化及后续更新都会被调用, 可以返回一个对象来更新 state, 如果返回 null 则不更新任何内容

getSnapshotBeforeUpdate(prevProps, prevState) {return null}

> 在第一次加载的时候不会执行, 在收到更新后, 在 render 之后 didUpdate 生命周期之前执行, 返回的值将在componentDidUpdate(prevProps, prevState, snapshot)作为第三个参数传递



### Hooks

useEffect

> 第二个参数是一个数组, 根据数组里的依赖项变化才会执行 effect 内的函数, 如果返回空, 类似 didMount 生命周期. 他的返回值是一个类似 willUnmount 执行的生命周期, 可以在这里返回一个函数



自定义 hook

> 需要以 use 开头, 函数内部可以调用其他 hook. 为了是逻辑复用
>
> hook 只能在函数最外层使用, 不能在**循环, 条件判断或者子函数中**调用
>
> 只能在函数组件中调用 hook 或者在 hook 中调用其他 hook

```react
//⾃自定义hook，命名必须以use开头 function useClock() {
  const [date, setDate] = useState(new Date());
  useEffect(() => {
console.log("date effect"); //只需要在didMount时候执⾏行行就可以了了 const timer = setInterval(() => {
setDate(new Date());
}, 1000); //清除定时器器，类似willUnmount
return () => clearInterval(timer);
}, []);
  return date;
}
```



useMemo, useCallback

> useMemo(callback, [依赖项]), 只有依赖项更改的时候才会重新执行 callback 函数
>
> 下面例子如果不加 useMemo 的话, 每次 inputvalue 值更新都会重新执行函数, 但是 expensive 的值其实只与 count 相关

```react
function UserMemoPage(props) {
	const [count, setCount] = useState(0)
  const expensive = useMemo(() => {
    let sum = 0;
    for (let i=0; i<count; i++) {
      sum+=i
    }
    return sum
  }, [count])
  const [value, setValue] = useState('')
  return (
  	<div>
    	<p>{count}</p>
      <p>依赖计算的值:{expensive}</p>
      <input value={value} onChange={e=>setValue(e.target.value)} />
    </div>
  )
}
```













