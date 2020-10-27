# 什么是JSX?
# 为什么要使用JSX?
# 怎样使用JSX?
## 在JSX中嵌入变量
````javascript
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>
ReactDOM.render(
  element,
  document.getElementById('root')
) 
````
***大括号中可以放入任意有效的Javascript表达式***  
*例如： 2+2, user.firstname, formate(user)*  
***
## JSX可视作JS对象
JSX会被Babel编译成普通JS函数调用，并返回JS对象  
也就是说，JSX可以在if, for, return等语句中使用  
***
## JSX特定属性
- 通过引号指定属性值为字符串字面量
- 通过大括号给属性赋JS表达式的值
````JSX
const element = <div tabIndex="0">;
const element = <img src={user.avatarUrl} />
````
*JSX语法上更接近Javascript而不是XML，所以ReactDOM使用camelCase命名*  
*class变成className, tabindex变成tabIndex*
***
## JSX多子元素

````JSX
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here. </h2>
  </div>
)
````
***
## JSX防止注入攻击
因此，你可以安全地在JSX中插入用户输入
````javascript
const title = response.potentiallyMaliciousInput;
const element = <h1>{title}</h1> // 直接使用是安全的
````
***
## JSX是个对象
Babel会把JSX转换成一个名为React.createElement()函数调用  
下述两种代码完全等效
````javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
)
````
````javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
)
````
createElement函数实际上创建了下面这样的对象
````javascript
// 这是简化后的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
}
````
**上述对象就叫做“React元素”，它描述了你希望在屏幕上看见的内容**