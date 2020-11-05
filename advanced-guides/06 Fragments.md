# Fragments
Fragments可以理解成：多个片段，多个部分（React元素返回的Fragments）  

在此之前，我们开发的组件 return 的都是单个React元素，想要返回多个元素就必须使用父元素包裹这些子元素，然后返回这个父元素。  

然而，这并不总是能满足要求的，例如在下述场景中，嵌套div将导致表格不能正常渲染  
````JSX
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    )
  }
}

class Columns extends React.Component {
  render() {
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  }
}

// 渲染后的 <Table />输出
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
````
Fragments的出现解决了这样的问题，它可以让React组件返回多个元素而不需要嵌套父元素  
## 用法
````JSX
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    )
  }
}
````
这样table组件就可以正常的渲染了！  
## 短语法
React提供了一种新的，更加简短的语法来声明Fragments，它看起来就是个空标签！
````JSX
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    )
  }
}
````
你可以像用其他标签一样来使用它，但**注意**：它不支持**key**和**属性**

# 带key的Fragments
使用显式的<code style="color:blue;"><React.Fragments></code>语法可以使用key属性。一个示例场景：将一个数组映射到一个Fragments数组，创建一个描述列表。
````JSX
function Glossary(props) {
  return (
    <div>
      {props.items.map(item => (
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </div>
  )
}
````
key是唯一可以传递给<code style="color:green;">Fragment</code>的属性，未来可能会添加其他属性，比如事件。
