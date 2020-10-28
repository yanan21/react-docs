# Steps to think in React
## 1. 根据UI划分组件层级
![划分示例](https://react.docschina.org/static/eb8bda25806a89ebdc838813bdfa3601/6b2ea/thinking-in-react-components.png)
- App
  - SearchBar
  - ProductTable
    - CategoryRow
    - ProductRow
  

## 2. 用React创建一个静态版本(渲染UI)
建议:
- 渲染UI和添加交互过程分开
- 通过props传入所需数据, 不推荐使用state构建静态版本  
  state代表了随时间变化而变化的数据, 应当仅在实现交互时使用
- 大型项目自下而上开发, 小型项目自上而下开发(先父后子)

## 3. 确定UI state的最小完整表示(添加交互)
想要UI具备交互能力, 需要触发基础数据改变的能力, React通过state来实现  
建议: 
- 只保留应用所需可变state的最小集合, 其他数据均由计算产生
## 4. 确定state的放置位置
寻找state放置组件的方法: 
- 找到根据该state进行渲染的所有组件(集合A)
- 找到他们的common owner组件, 在层级上高于所有集合A组件层级
- 该common owner组件应拥有该state
- 如果找不到合适位置存放state, 就再创建一个新的父组件来存放state
## 5. 添加反向数据流
反向数据流用于较低层级组件更新较高层级组件的state  
实现方法: 
- 将父组件实施更改local state的函数通过props传递给子组件
- 子组件监听用户时间，将props收到的更改函数作为eventHandler