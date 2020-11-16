# 受控组件
什么是受控组件与非受控组件？Controlled components & Uncontrolled components

- 非受控组件：HTML原生表单元素, 例如input, textarea, select等
- <u>**受控组件**</u>  
  React组件内部state成为表单组件的唯一数据源  
  组件控制着/处理用户操作(即管理事件处理)  
  被React以这种方式控制取值的表单输入元素称为"受控组件";  
  与Vue中data作为表单元素唯一数据源、methods处理用户事件是类似的  

选择受控组件与否的建议:  
- 在不能使用受控组件的时候选择作为替代品的非受控组件
- 成熟的解决方案: [formik][formik], 包含验证, 追踪访问字段, 处理表单提交
***
### input, textarea, select
- textarea元素  
  在HTML中, textarea元素通过子元素定义其文本(标签之内)
  在React中, textarea使用value属性代替, 与input类似
- select元素: React中同样使用value属性指定/绑定选定值

***
### handle multiple input
处理多个输入表单元素时的解决办法:
1. 对不同表单元素编写不同eventHandler
2. 使用相同eventHandler, 根据表单元素name属性执行不同操作
***
### 阻止用户更改
在受控组件上指定value为特定值, 将组织用户更改输入  
如果指定value后,用户仍能编辑, 可能是你意外地将value设置为undefined/null
# 非受控组件
## input[type=file]
上传文件的表单元素是个非受控组件,因为其value是只读的  
React对非受控组件的处理, 详情可见[非受控组件](../advanced-guides/21%20Uncontrolled%20Components.md)
***

本节示例可在<code style="color:red;">myapp>form</code>中查看

***
[上一篇：列表渲染](./08%20Lists%20and%20Keys.md)
<u style="float:right;">[下一篇：状态提升](./10%20Lifting%20State%20up.md)</u>










[formik]: https://jaredpalmer.com/formik