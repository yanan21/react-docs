# 错误边界
## 为什么需要？
在React16之前，组件内的JavaScript错误会导致React的内部状态被破坏，并且在下一次渲染时产生无法追踪的错误。这些错误基本上是由其他的非React组件代码错误引起的。但React并没有提供一种优雅的错误处理，也无法从错误中恢复。  

部分UI的JS错误不应该导致整个应用的崩溃，为了解决这个问题，React引入了一个新的概念——错误边界。  

错误边界是一种React组件，这种组件可以捕获并打印发生在其子组件数任何位置的JS错误，然后，渲染出备用UI，而非崩溃了的子组件。错误边界可以在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。  
> **注意**  
> 错误边界无法捕获以下场景中产生的错误  
> - 事件处理
> - 异步代码
> - 服务端渲染
> - 自身错误
***
## 怎样创建一个错误边界组件？
1. 创建一个Class组件(只可以是Class组件)
2. 在Class组件中定义<code>static getDerivedStateFromError()</code>或<code>componentDidCatch()</code>这两个声明周期方法中的任意一个/两个，那么这个组件就是一个错误边界组件了。

````JSX
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError(error) {
    // 更新 state 使下一次渲染显示降级后UI
    return { hasError: true}
  }

  componentDidCatch(error, errorInfo) {
    // 可以将错误信息上报给服务器
    logErrorToServer(error,errorInfo)
  }

  render() {
    if(this.state.hasError) {
      return <h1>渲染降级后的UI界面</h1>
    }
    return this.props.children
  }
}
````
抛出错误后，使用<code>getDerivedStateFromError()</code>渲染备用UI，使用<code>componentDidCatch()</code>打印错误信息  

然后你就可以将它作为一个常规组件来使用了
````JSX
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
````
> 任何未被错误边界捕获的错误将会导致组件树被卸载，移除好过错误。
***
## 错误边界应该放在哪儿？
错误边界的粒度是开发者决定的，你可以将错误边界放在最顶层，也可以用于捕获特定的组件错误，以保护其他部分不崩溃。
***
## 错误边界与try...catch
异同点：  
- try/catch是命令式代码(imperative code)
- 错误组件是(React组件)是声明式的
- 错误边界只针对React组件，只有class组件可以成为错误边界组件
- 错误边界只能捕获子组件错误，若不能渲染错误，则错误会冒泡至最近的上层错误边界，与catch类似

***
## 关于事件处理
错误边界是无法捕获事件处理器内部错误的。   
React也不需要错误边界来捕获时间处理器内部的错误。与render方法和生命周期方法不同，事件处理函数不会在渲染期间触发，因此如果你需要在事件处理器内部捕获错误，直接使用不同的try/catch语句就可以了。
***
> 本节示例程序见: <code>myapp>src>components>ErrorBoundary</code>