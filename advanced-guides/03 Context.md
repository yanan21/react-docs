# 简要介绍
在典型的React应用中，数据是通过props属性自上而下（由父及子）进行传递的  
但这种做法对于类似地区偏好，UI主题等全局共享的属性而言是极其繁琐的，这类属性在应用中会被许多组件使用  
Context提供了一种在组件之间共享此类值的方式，而无需逐层传递props
***
# 何时使用
Context的设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主体或者首选语言
````JSX
// Class组件使用Context
const ThemeContext = React.createContext('light')

class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value='dark'>
        <Toolbar />
      </ThemeContext.Provider>
    )
  }
}
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  )
}
class ThemedButton extends React.Component {
  static contextType = ThemeContext
  render() {
    return <Button theme={this.context}>
  }
}
````
# 你也许不需要Context
Context的主要应用场景在于很多不同层级的组件需要访问同样一些数据。  
应该谨慎使用，因为使用Context会让降低组件的复用性。

如果只是想避免层层传递一些属性(某个深嵌组件需要使用某个属性)，组件组合(Component Compostion)是个比context更好的解决方案。  

例如，你有一个Page组件，它层层向下传递user和avatarSize属性，从而深度嵌套的Link和Avatar组件可以使用这些属性  
````Javascript
<Page user={user} avatarSize={avatarSize} />
// ...子组件...
<PageLayout user={user} avatarSize={avatarSize} />
// ...子组件...
<NavigationBar user={user} avatarSize={avatarSize} />
// ...子组件...
<NavigationBar user={user} avatarSize={avatarSize} />
// ...渲染出...
<Link href={user.permalink}>
  <Avatar user={user} size={avatarSize}>
</Link>
````
像上面这种最终只有Avatar组件真的需要user和avatarSize属性的，那么层层传递这两个props就显得非常冗余，而且一旦Avatar组件需要更多来自顶层组件的props，你还要在中间层一个个加上去，非常麻烦。  

一种**无需Context**的方法就是使用“<u>组件组合</u>”：将Avatar组件自身传递下去，因而中间件无需知道user或者avatarSize等props  
````JSX
function Page(props) {
  const user = props.user
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={avatarSize} />
    </Link>
  )
  return <PageLayout userLink={userLink} />
}
<Page user={user} avatarSize={avatarSize} />
<PageLayout userLink={...}>
<NavigationBar userLink={...}>
{props.userLink}
````
并且组件不限制接受子组件的个数，你可以像“**组件组合**”中说的那样为子组件封装多个单独的接口(slots)
````JSX
function Page(props) {
  const user = props.user
  const content = <Feed user={user} />
  const topBar = (
    <NavigationBar>
      <Link href={user.permalink}>
        <Avatar user={user} size={avatarSize} />
      </Link>
    </NavigationBar>
  )
  return (
    <PageLayout
      topBar={topBar}
      content={content}
    />
  )
}
````
“组件组合”模式已经可以覆盖大多场景了，但如果子组件在渲染前需要和父组件进行交互，你可以进一步使用[<u>render props</u>](./17%20Render%20Props.md)

优点：
- 减少了组件中传递的props数量
- 代码更加简洁干净 
 
缺点：
- 这种将逻辑提升到组件树更高层会使高层组件复杂
***
# API
## React.createContext
````javascript
const MyContext = React.createContext(defaultValue)
````
上述语句创建了一个Context对象，当组件A订阅了这个Context时，A会从组件树中距离自身最近的那个匹配的Provider中读取当前context值  

只有当组件所处的树中没有匹配到Provider时，defaultValue才会生效。这有助于在不适用Provider包装组件的情况下对组件进行测试。若将undefined传递给Provider的value，consumer组件的defaultValue不会生效。
## Context.Provider
每个Context对象都会返回一个Provider组件，它允许Consumer组件订阅Context的变化  
Provider接收一个value属性，传递给Consumer组件，一个Provider可以和多个Consumer组件有对应关系。多个Provider也可以嵌套使用，里层的会覆盖外层的数据。  

当Provider的value值变化时，其内部所有Consumer组件都会重新渲染。  
Provider及其内部Consumer组件不受制于shouldComponentUpdate函数  
因此Consumer组件在其祖先节点退出更新时也可以更新  
> 新旧值检测使用了Object.is相同的算法  
> 当value是对象时，可能会倒置一些问题，详见官网
## Context.Consumer
````JSX
<MyContext.Consumer>
  {value => (
    /* something here...*/
  )}
</MyContext.Consumer>
````
上述是通过<u>[function as a child](17%20Render%20Props.md)</u>做法让函数式组件订阅Context.  
这个函数接收当前的Context值返回一个React元素,value等于Provider的value
## Context.displayName
````JSX
const MyContext = React.createContext(defaultValue)
MyContext.displayName = 'DemoContext'
````
如上，context对象接受一个String类型、名为displayName的property，用以在React DevTools中显示。
## Class.contextType
````Javascript
class Demo extends React.Component {
  componentDidMount() {
    let value = this.context
    /* 组件挂载后 根据MyContext组件值执行一些操作 */
  }
  componentDidUpdate() {
    let value = this.context
    /* ... */
  }
  componentWillUnmount() {
    let value = this.context
    /* ... */
  }
  render() {
    let value = this.context
    /* 基于 MyContext value进行渲染 */
  }
}
Demo.contextType = MyContext
````
挂载在class上的contextType属性会被重新赋值为context对象  
这让你可以使用<code>this.context</code>来读取context的值，你可以在任何声明周期内访问它。
> 如果你在使用 public class fields 语法，可以使用 static 初始化 contextType

````Javascript
class Demo extends React.Component {
  static contextType = MyContext
  render() {
    let value = this.context
    /* 基于value渲染 */
  }
}
````
# 嵌套组件中更新Context
想要在嵌套很深的组件中欧更新context，只需要通过context传递一个函数，深嵌组件即可调用函数更新context
````JSX
export const ThemeContext = React.createContext({
  theme: themes.dark,
  toggleTheme: () => {}
})

function ThemeBtn() {
  return (
    <ThemeContext.Consumer>
      {({theme, toggleTheme}) => (
        <button onClick={toggleTheme} style={{backgroundColor: theme.background}}>
          Toggle Theme
        </button>
      )}
    </ThemeContext.Consumer>
  )
}
````
> 示例见<code>myapp>src>components>context</code>  
***
# 使用多个Context
````JSX
const ThemeContext = React.createContext('light')
const UserContext = React.createContext({
  name: 'Guest'
})

class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props
    // 提供初始 Context 值的 App 组件
    return (
      <ThemeContext.Provider>
        <UserContext.Provider>
          <Layout />
        </UserContext.Provider>
      </ThemeContext.Provider>
    )
  }
}

function Layout() {
  return (
    <div>
      <SideBar />
      <Content />
    </div>
  )
}

function Content() {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <ProfilePage user={user} theme={theme} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  )
}
````
> 示例见<code>myapp>src>components>context</code>  

若两个或者多个context值经常一起使用，就要考虑另外创建一个渲染组件来提供该值。
***
# 注意事项
context使用参考标识(reference identity)来决定何时进行渲染，这种做法可能会造成如下问题：  
当Provider的父组件重新渲染时，consumers组件会触发意外渲染。如下，value属性总被赋值为新的对象  
````JSX
class App extends React.Component {
  render() {
    return (
      <MyContext.Provider value={{something: 'something'}}>
        <ToolBar />
      </MyContext.Provider>
    )
  }
}
````
为了防止这种情况，将value状态提升到父节点的state中去
````JSX
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      value: {something: 'something'}
    }
  }
  render() {
    <MyContext.Provider value={this.state.value}>
      <ToolBar />
    </MyContext.Provider>
  }
}
````
> 示例见<code>myapp>src>components>context</code>  
