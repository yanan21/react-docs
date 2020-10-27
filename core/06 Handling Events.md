# 1. 事件处理的不同点
- React事件的命名采用小驼峰camelCase方式，而非纯小写
- 使用JSX语法时传入函数作为时间处理函数，而非字符串
````html
<!-- HTML -->
<button onclick="activateLasers()">
  Activate Lasers
</button>
<!-- REACT -->
<button onClick={activateLasers}>
  Activate Lasers
</button>
````
- React中不能通过return false阻止默认行为 需要显示preventDefault
````Javascript
// 函数组件
function ActionBtn() {
  function handleClick(e) { // e是符合W3C规范定义的合成事件 不需要担心浏览器兼容问题
    e.preventDefault()
    console.log('button clicked')
  }
  <button onClick={handleClick}>Click me</button>
}
// class组件
class Toggle extends React.Component {
  constructor(props) {
    super(props)
    this.state = {isToggleOn: true}
    this.handleClick = this.handleClick.bind(this)
  }
  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }))
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    )
  }
}
````
**注意**：在使用class组件的时候要绑定this，否则this会是undefined
## 不绑定this的几种办法
- 使用实验性的public class fields语法(create-react-app默认启用此语法)
````Javascript
class LoginBtn extends React.Component {
  handleClick = () => {
    console.log('this is: ' + this)
  }
  render() {
    return (
      <button onClick={this.handleClick}>Login</button>
    )
  }
}
````
- 回调中使用箭头函数
````Javascript
class LoginBtn extends React.Component {
  handleClick() {
    console.log('this is: ' + this)
  }
  render() {
    return (
      <button onClick={() => this.handleClick()}>Login</button>
    )
  }
````
第二种(箭头回调)方法问题在于：每次渲染组件LoginBtn的时候都会创建不同的回调函数  
在大多数情况下，这并不是什么问题  
但如果该函数作为prop传入子组件时，子组件可能会因此进行额外的重新渲染  
建议在构造器中绑定this或者使用class fields语法避免性能问题

# 2. 向事件处理程序传递参数
在实际开发中，事件处理函数通常需要传入参数，比如数据的删除操作等  
这就要求我们给事件处理函数传入参数  
传递参数的方式有两种  
````Javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>  
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
````
上述分别使用“箭头函数”和“Function.prototype.bind”来实现，效果等价  
共同点在于: 两种方式，React事件对象e都会作为蝶儿个参数传递，箭头显示，bind隐式
# 3. 事件处理的一般示例
详见myapp>LoginBtn.jsx组件