# 1. React元素与DOM元素
- React元素是构成React应用的最小砖块
- React元素是开销极小的普通对象
- React组件是由React元素构成的  
**React DOM负责更新DOM来与React元素保持一致**
***
# 2. React元素渲染成DOM元素
````html
<div id="root"></div>
````
称上述div节点为根DOM节点，因为该节点内所有内容都将由React DOM管理  
要将React元素/组件渲染到根DOM节点中，只需要将其传入ReactDOM.render()  
````javascript
const element = <h1> Hello, world! </h1>;
ReactDOM.render(element, document.getElementById('root'));
````
***
# 3. 更新已渲染元素
## 怎样更新？
- first thing first: React元素是不可变的对象，once created, can't be modified
- 也就是说一个React元素就像电影的一帧似的，代表的是某个特定时刻的UI
- 所以，更新UI的唯一方式就是创建一个新的React元素再传入ReactDOM.render()  
- 比如计时器的例子，每秒钟都调用ReactDOM.render()
- ***实际上大多数React应用只会调用一次ReactDOM.render()***
- ***上述案例可以通过有状态组件实现***
## 部分更新
- ReactDOM负责更新DOM与React元素保持一致
- ReactDOM会将React元素和其子元素与他们之前的状态进行比较
- 只更新必要的DOM元素
- 这种减少DOM元素操作的行为使得网页加载更快(DOM操作慢)
- 尽快React元素更新时每次都会创建一个新的描述整个UI树的对象
- 但ReactDOM只会更新实际改变了的内容