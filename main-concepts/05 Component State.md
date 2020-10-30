# 1. 使用State更新渲染DOM
- 03 React element 更新渲染一节中提到  
- 通过设置定时器重复调用ReactDOM.redner()渲染是不推荐的做法
- 本节将通过State编写有状态的组件完成之前效果
````JSX
class Clock extends React.Component {
  constructor(props) {
    super(props)
    this.state = {date: new Date()}
  }
  componentDidMount() {
    this.timerID = setInterval(() => {
      this.tick(),
      1000
    })
  }
  componentWillUnmount() {
    clearInterval(this.timerId)
  }
  tick() {
    this.setState({
      date: new Date()
    })
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}</h2>
      </div>
    )
  }
}

ReactDOM.render() {
  <Clock />,
  document.getElementById('root')
}
````
# 2. State的正确使用
## 不要直接修改State
````javascript
this.state.comment = 'hello' // Wrong
this.setState({comment: 'hello'}) // Correct
````
*构造函数是唯一可以给this.state赋值的地方*
## State更新的浅合并
- 在调用setState()时，React会把你提供的对象合并到当前的state
- 这里的合并是浅合并，新值会完全替换当前值
````JSX
this.state = {
  posts: [],
  comments: []
}
componentDidMount() {
  fetchPosts().then(res => {
    this.setState({
      posts: res.posts
    })
  })
  fetchComments().then(res => {
    this.setState({
      comments: res.comments
    })
  })
}
````
## State的更新可能是异步的
- 为了提升性能，React会把多个setState()调用合并成一个调用
- 这就意味着this.props和this.state可能会异步更新
- 所以不要依赖他们当前值来更新下一个状态

````JSX
// 错误做法
this.setState({
  counter: this.state.counter + this.props.increment
})

// 正确做法
this.setState((state, props) => {
  counter: state.counter + props.increment
})
````
# 3. 理解组件的"单向"数据流
- 组件之间不知道彼此是否有状态，也不关心
- state之所以被称作局部的/封装的，是因为只可以自己访问
- 组件可以将其state作为props传给其子组件
````html
<FormattedDate date={this.state.date} />
````
- FormattedDate组件会在props中接收参数date，但不知其来自父组件的state?输入
- **上述通常被称作“自上而下单向”的数据流**
- 任何state总属于特定的组件，只能响应低于他们的组件
- 将组件树想象成一个props的数据瀑布
- 每一个组件的state就像是在任意一点给瀑布添加额外的水源，但只能往下流动