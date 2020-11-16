## 条件渲染：根据条件渲染不同组件/组件不同元素
# 1. if
````JSX
function Usergreeting(props) {
  return <h1>Welcome back!</h1>
}
function Guestgreeting(props) {
  return <h1>Please sign in.</h1>
}

function Greeting(props) {
  const isLoggedIn = props.isLoggedIn
  if(isLoggedIn) {
    return <Usergreeting />
  }
  return <Guestgreeting />
}

ReactDOM.render(
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
)
````
# 2. 渲染部分子组件/React元素
使用变量来存储组件或React元素，可以让你有条件地渲染部分组件
````JSX
// 登录按钮组件
function LoginBtn(props) {
  return (
    <button onClick={props.onClick}>Login</button>
  )
}
// 登出按钮组件
function LogoutBtn(props) {
  return (
    <button onClick={props.onClick}>Logout</button>
  )
}
````
条件渲染组件
````JSX
class LoginControl extends React.Component {
  constructor(props) {
    super(props)
    this.handleLoginClick = this.handleLoginClick.bind(this)
    this.handleLogoutClick = this.handleLogoutClick.bind(this)
    this.state = {isLoggedIn: flase}
  }
  handleLoginClick() {
    this.setState({isLoggedIn: true})
  }
  handleLogoutClick() {
    this.setState({isLoggedIn: false})
  }
  render() {
    const isLoggedIn = this.state.isLoggedIn
    let button
    if(isLoggedIn) {
      button = <LogoutBtn onClick={this.handleLogoutClick} />
    } else {
      button = <LoginBtn onClick={this.handleLoginClick} />
    }
    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn}>
        {button}
      </div>
    )
  }
}
````
# 3. && 运算符
first thing first: in <u>Javascript</u>  
- **true && expression => expression**
- **false && expression => false**
````Javascript
function Mailbox(props) {
  const unreadMessages = props.unreadMessages
  return (
    <h1>Hello</h1>
    {unreadMessages.length >0 &&
      <h2>
        You have {unreadMessages.length} unread messages.
      </h2>
    }
  )
}
const messages = ['React', 'Re: React', 'Re:Re: React']
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
)
````
# 4. 三目运算符
````JSX
render() {
  const isLoggedIn = this.state.isLoggedIn
  return (
    <h2>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b>logged in.
    </h2>
    <div>
      {isLoggedIn
        ? <LogoutBtn onClick={this.handleLogoutClick} />
        : <LoginBtn onClick={this.handleLoginClick} />
      }
    </div>
  )
}
````
# 5. 阻止组件渲染
- 函数组件直接<code>return null</code>
- class组件让render函数<code>return null</code>  

详情示例见：<code style="color:red;">myapp>WarningComponent</code>

***
[上一篇：事件处理](./05%20Component%20State.md)
<u style="float:right;">[下一篇：列表渲染](./08%20Lists%20and%20Keys.md)</u>
