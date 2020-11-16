# 组件组合
React有着十分强大的组合模式, 用以实现组件间代码的重用  
组件间关系决定着组件之前的组合关系
## 包含关系
对于例如侧边栏(Sidebar), 对话框(Dialog)这种通用容器来说，我们无法提前知晓其子组件的具体内容    
**1. 使用特殊的prop: children prop来把子组件传递到渲染结果中去。**
````JSX
function Sidebar(props) {
  return (
    <div style={{color: props.color}}>
      {props.children}
    </div>
  )
}

function Layout(props) {
  return (
    <div>
      <Sidebar color="red">
        <h1>Sidebar title</h1>
        <p>Navigation links here...</p>
        <AnotherComponent />
      </Sidebar>
    </div>
  )
}
````
**2. 将组件通过props传递给父组件**

- 组件等React元素本质上就是对象，因此完全可以作为props传递  
- 这给你一种Vue中插槽(Slot)的感觉，但React中不存在插槽这一限制  
- 你可以将任何东西作为props传递  
````JSX
function Pane(props) {
  return (
    <div className="pane">
      <div className="left-pane">{props.left}</div>
      <div className="right-pane">{props.right}</div>
    </div>
  )
}

function App() {
  return (
    <Pane left={<Chat />} right={<Contact />} />
  )
}
````
## 特例关系
- 所谓特例关系，即组件B是组件A的特例形式 通过对A进行封装实现B
- 类似于B是A类的实例化，通过props(构造函数)进行封装(实例)

````JSX
// 抽象组件A
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1>{props.title}</h1>
      <p>{props.message}</p>
    </FancyBorder>
  )
}
// 具化组件B
function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank your for focusing!" />
  )
}
````
***
# 那继承呢?
没有继承！一切都可以用组合实现，不需要继承！

***
[上一篇：状态提升](./10%20Lifting%20State%20up.md)
<u style="float:right;">[下一篇：React思维](./12%20Thinking%20in%20React.md)</u>