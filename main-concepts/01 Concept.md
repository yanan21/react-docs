# React是什么
React是一个**声明式**的用于构建用UI的JavaScript库
# React特点
- 声明式：命令式？
- 组件化
- 一次学习 随处编写
# 怎样学习入门
- 边学边做：[实践教程](https://react.docschina.org/tutorial/tutorial.html)
- 概念学习：[教程文档](https://react.docschina.org/docs/getting-started.html)
# 创建demo的方式
1. CDN方式引入unpkg的包: 示例见<code style="color:red;">first-demo</code>
2. 使用集成的工具链

推荐的工具链
- 学习/创建一个新的SPA时：使用 Create React App
- 在用Node.js构建SSR网站时：试试Next.js
- 在构建面向内容的静态网站时：试试Gatsby
- 打造组件库/将React集成到现有代码仓库：尝试更灵活的工具链
### Create React App
create-react-app 非常适用学习React以及创建SPA  
它会自动配置开发环境，为生产环境优化程序  
使用要求：node >= 8.10 && npm >= 5.6  
````shell
npx create-react-app myapp
cd myapp
npm start
````
<u>npx是npm5.2+附带的package运行工具</u>  
create-react-app创建一个前端构建流水线(build pipeline) 不会处理后台逻辑/操作数据库  
通过上述命令的执行，可以获得一个可执行的React项目
***
[上一篇：文档导航](./beforeTutorial.md)
<u style="float:right;">[下一篇：JSX介绍](02%20JSX%20Intro.md)</u>