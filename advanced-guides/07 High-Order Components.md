# 高阶组件
HOC 并不是 React API 的一部分，而是一种基于React组合特性而形成的设计模式，是复用组件逻辑的一种高级技巧。  

一般组件是将props转换成UI，高阶组件是将**组件**作为参数，返回新组件的函数。  
````javascript
const EnhancedComponent = higherOrderComponent(WrappedComponent)
````
HOC 在React的第三方库中很常见，例如 Redux 的<code>connect</code>和 Relay的<code>createFragmentContainer</code>
## 使用 HOC 解决横切关注点问题
什么是横切关注点？ cross-cutting concern ?

举栗子:  
CommentList 和 BlogPost 组件有如下共性：  
- 在挂载时都需要向<code>DataSource</code>添加一个更改侦听器  
- 在侦听器内部，数据源改变时调用<code>setState</code>
- 在卸载时删除侦听器

在大型应用中，这种订阅<code>DataSource</code>和调用<code>setState</code>的模式会一次又一次地发生。此时，我们就需要一个抽象，在一个地方集中的定义这个逻辑，让多个组件直接使用它来订阅某个数据源。  

````JSX
// 使用前
class CommentList extends React.Component {
  constructor(propr) {
    super(props)
    this.handleChange = this.handleChange.bind(this)
    this.state = {
      comments: DataSource.getCommentss()
    }
  }
  componentDidMount() {
    DataSource.addChangeListener(this.handleChange)
  }
  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange)
  }
  handleChange() {
    this.setState({
      comments: DataSource.getComments()
    })
  }
  render() {
    return (
      <div>
        {this.state.comments.map(comment => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    )
  }
}
````
````JSX
// 使用后
const CommentListWithSubscription = withSubscription(
  CommentList,
  DataSource => DataSource.getComments()
)

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
)
````
````JSX
// HOC  
function withSubscription(WrappedComponent, getData) {
  return class extends React.Component {
    constructor(props) {
      super(props)
      this.state = {
        data: getData(dataSource, props)
      }
    }
    componentDidMount() {
      dataSource.addEventListener(this.handleChange)
    }
    componentWillUnmount() {
      dataSource.removeEventListener(this.handleChange)
    }
    handleChange = () => {
      this.setState({
        data: getData(dataSource, props)
      })
    }
    render() {
      return <WrappedComponent data={this.state.data} {...this.props} />
    }
  }
}
````
***
## 纯函数，不改变原组件，使用组合
HOC 是纯函数，不会修改传入的组件，也不会通过继承来赋值其行为，相反，HOC 将组价你包装在容器组件中来组成新组件。

不要试图在 HOC 中修组件原型（或其他方式修改），这样做的不良后果包括：输入组件无法像HOC增强前那样使用、再用另个一个 HOC 修改相同部分，之前的就会失效！

HOC 不应该修改传入组件，而应该使用组合将传入组件包装在容器组件中实现功能。
***
*以下内容待进一步学习！*
## 约定

**将不相关props传递给被包裹组件**

**最大化可组合性**

**包装显示名称以便轻松调试**

## 注意
**不要在render方法中使用HOC**

**务必赋值静态方法**

**Refs不会被传递**