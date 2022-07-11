[文档链接](https://react.docschina.org/docs/lifting-state-up.html)  6-12

### 6事件处理函数

- 必须传入函数

  > eg:
  >
  > <button onClick={() => this.handleClick()} />
  >
  > 错误:
  >
  > <button onClick="this.handleClick()" />

  

- 需要绑定 this  或者通过 [public-class-fields](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/Public_class_fields) 语法

  > ```react
  > class LoggingButton extends React.Component {
  > 	constructor(props) {
  > 		super(props)
  > 		this.handleClick = this.handleClick.bind(this)
  > 	}
  > 	//获取不到 this
  >   handleClick() {
  >     console.log('this is:', this);
  >   }
  > 	handleClick = () => {
  >     console.log('this is:', this);
  >   }
  >   render() {
  >     // 此语法确保 `handleClick` 内的 `this` 已被绑定。    return (      <button onClick={() => this.handleClick()}>        Click me
  >       </button>
  >     );
  >   }
  > }
  > 
  > ```

  

- 避免重复渲染 尽量使用[public-class-fields](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/Public_class_fields) 语法 或者绑定 this 的方式

  >render() {
  >
  >​	handleClick = () => {
  >
  >​	// 这样不会导致重复渲染
  >
  >​	}
  >
  >​	// 此语法确保 `handleClick` 内的 `this` 已被绑定。  
  >
  >​	return (
  >
  >​		<button onClick={() => this.handleClick()}>
  >
  >​			Click me
  >​      	</button>
  >​    );
  >  }

- 传递参数

  > ```react
  > <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
  > <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
  > ```



### 7.条件渲染

1. if else 选择组件
2. 三木运算 length > 0 ? <A /> : <B />
3. true && expression 比如   length>0 && <AComponent />

阻止渲染: 只要 render 返回 null

### 8.列表渲染&key

> 列表里的每一项都需要有一个 key, 最好是用数据的 id 来作为 key, 如果用下标 index 来作为key 的话, 可能会引起一些[问题](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)以及为什么 [key 是必须的](https://react.docschina.org/docs/reconciliation.html#recursing-on-children)
>
> key 必须是兄弟节点间唯一,  不需要是全局唯一. 当我们生成两个不同的数组时，我们可以使用相同的 key 值
>
> key 不会再组件里显示传递, 如果需要传递 key 的值,请通过别的字段显示传递

```react
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>{number}</li>
);
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```



### 表单

select, 有一个原生属性为 selected 可以默认选中, 但是 react使用根 select 元素上的 value 属性来控制谁被选中

多选:

> ```react
> <select multiple={true} value={['B', 'C']}>
> ```

处理多个 input, 可以在 input 标签上使用 name 属性区分元素



受控组件输入空值 null, 此时受控组件变为可输入组件

```react
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);

```



### 10.状态提升

在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。这就是所谓的“状态提升”。



### 11.组合,继承

- children, 所有组件内所包含的都会通过 children 属性传入

- 类似 slot 的组件可以通过 props 里的属性传入如下:

  ```react
  function SplitPane(props) {
    return (
      <div className="SplitPane">
        <div className="SplitPane-left">
          {props.left}
        </div>
        <div className="SplitPane-right">
          {props.right}
        </div>
      </div>
    );
  }
  
  function App() {
    return (
      <SplitPane
        left={
          <Contacts />
        }
        right={
          <Chat />
        } />
    );
  }
  ```



### 12REACT 哲学

