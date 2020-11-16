### Array.map()函数
````JavaScript
const numbers = [1, 2, 3, 4, 5]
const doubled = numbers.map(number => number *2)
````
同样地，通过数组的map函数将数组转换成React元素列表
***
# 1. 组件中渲染列表
````JSX
function NumberList(props) {
  const numbers = props.numbers
  const items = numbers.map(number => {
    <li key={number.toString()}>{number}</li>
  })
  return (
    <ul>{ items }</ul>
  )
}
````
**or**
````JSX
function NumberList(props) {
  const numbers = props.numbers
  
  return (
    <ul>
    {numbers.map(number => {
      <li key={number.toString()}>{number}</li>
      })
    }</ul>
  )
}
````
# 2. key
- key能够让React识别哪些元素改变/删除/添加了，必须给每个元素赋予唯一标识
- 独一无二的key,通常使用id
- 万不得已可以用index作为key，这也是不声明时React的默认选择
- 使用index作为key时，可能会倒置性能变差，引起组件状态问题
- key的声明必须在数组上下文中才有意义
- ke只是在兄弟节点之间比唯一，与其他节点无关联无影响

***
[上一篇：条件渲染](./07%20Conditional%20Rendering.md)
<u style="float:right;">[下一篇：表单](./09%20Forms.md)</u>