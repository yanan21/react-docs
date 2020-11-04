## 打包
大多数React应用都会使用Webpack/Rollup/Browserify这类构建工具来打包文件  
打包指的是将引入文件合并到一个单独文件的过程，最终形成一个bundle，接着在页面内引入该bundle，就可以一次性载入！  
如果你在使用 Create React App, Next.js, Gatsby等类似工具，你会拥有一个可以直接使用的Webpack配置来进行打包工作。  
如果没有使用上述工具，就需要自己进行配置  
***
# 代码分隔
尽管打包是个很棒的技术，但随着应用的增长和第三方库的增加，难免会出现页面加载缓慢现象  
为避免搞出大体积代码包，前期考虑到代码分隔是明智的  
常见的代码构建工具/打包器都支持这一技术，能够创建多个包并在运行时动态加载。  
<u>代码分隔可以让你“懒加载”用户所需内容，提升响应速度</u>
## 动态import语法
动态import()语法是引入代码分隔的最佳方式  
````Javascript
import('./math').then(math => {
  console.log(math.add(19, 21))
})
````
Webpack在解析到该语法时，会自动进行代码分隔  
<u>当你使用Babel时，要确保Babel能解析动态import语法，而非进行转换，你需要**babel-plugin-syntax-dynamic-import插件**</u>
***
## React.lazy
React.lazy函数能让你像渲染常规组件一样处理动态引入的组件
````Javascript
// not using React.lazy
import OtherCompo from './OtherCompo'

// using React.lazy
const OtherCompo = React.lazy(() => import('./OtherCompo'))
````
React.lazy接收一个函数，该函数要动态调用import()，且必须返回一个Promise，该Promise需要resolve一个default export的React组件  

**接下来**，应该在Suspense组件中渲染lazy组件，如此使得我们可以做组件加载中的优雅降级，即loading指示器  
````Javascript
import React, { Suspense } from 'react'

const OtherCompo = React.lazy(() => import('./OtherCompo))

function MyCompo() {
  return (
    <div>
      <Suspense fallback={<div>Loading....</div>}>
        <OtherCompo />
      </Suspense>
    </div>
  )
}
````
fallback属性接收任何React元素  
Suspense组件可以置于懒加载组件之上的任意位置(任意父级别)  
Suspense组件可以包裹多个懒加载组件  
***
## 异常捕获边界(Error boundaries)
如果模块加载失败(如网路问题)，它会触发一个错误  
你可以通过异常捕获边界(Error boundaries)来处理，以显示良好的用户体验并管理恢复事宜  
````Javascript
import React, { Suspense } from 'react'
import MyErrorBoundary from './MyErrorBoundary'

const OtherCompo = React.lazy(() => import('./OtherCompo'))
const AnotherCompo = React.lazy(() => import('./AnotherCompo'))

const MyCompo = () => {
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherCompo />
          <AnotherCompo />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
}
// 具体实现见 04 Error Boundarires 
````
***
## 基于路由的代码分隔
决定在哪儿使用“引入代码分隔”是需要一些技巧的  
引入代码分隔要以均匀分隔、不影响用户体验为前提条件  

大多数用户习惯网页切换过程，所以常见的选择是从路由开始  
````Javascript
import React, { Suspense, lazy } from 'react'
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'

const Home = lazy(() => import('./routes/Home'))
const About = lazy(() => import('./routes/About'))

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Suspense>
  </Router>
)
````
***
## 命名导出(Named Exports)
React.lazy目前只支持默认导出(default exports)  
如果想要被引入的模块使用命名导出(named exports)  
你可以创建一个中间模块，用以导出为默认模块，来保证tree shaking不出错  
````Javascript
// Messages.js
export const AllMessages = /*...*/
export const UnreadMessages = /*...*/

// UnreadMessages.js
export { UnreadMessages as default } from './Messages.js'

// App.js
import React, { lazy } from 'react'
const UnreadMessages = lazy(() => import('./UnreadMessages'))
````
***
> 代码分割示例见：<code>myapp>src>components>LazyCompo</code>