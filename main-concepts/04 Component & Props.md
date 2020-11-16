# 1. 组件定义
- 组件将UI拆分成多个独立可复用的代码片段
- 概念上类似于JS函数，接收任意入参(props)，返回React元素
# 2. 组件类型
## 函数组件
```JSX
function Welcome(props) {
  return <h1> Hello, {props.name} </h1>;
}
```
## class组件
````JSX
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name} </h1>;
  }
}
````
# 3. 组件渲染
React元素不限是DOM元素，也可以是用户自定义组件
````JSX
const element = <Welcome name="sara" />
ReactDOM.render(
  element,
  document.getElementById('root')
)
````
- **当React元素是用户自定义组件时，它会将JSX接收到的属性(attribute)以及子组件(children)转换为单个对象(props)传递给组件**
- **React组件名称必须以大写字母开头，小写字母开头组件会被视作DOM标签**
# 4. 组合组件
可以在父组件的输出中引用其他组件，这样就可以抽象出多层次
````JSX
function App () {
  return (
    <div>
      <Header />
      <Main />
      <Footer />
    </div>
  )
}
````
*相反，当某个组件过大/过于复杂时，可以将组件不同部分抽象提取出来*
# 5. Props的只读性
- **无论是函数组件，还是类组件都决不能修改自身的props**
- **所有React组件必须像纯函数一样保护其props不被改变**
- **在不违反上述规则的情况下，State允许React组件根据用户操作、网络响应、动态更改输出内容**

***
[上一篇：React元素](./03%20React%20Element.md)
<u style="float:right;">[下一篇：组件状态](./05%20Component%20State.md)</u>