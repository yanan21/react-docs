# 核心概念
## state
````javascript
{
  todo_list: [
    { text: '吃饭', completed: true },
    { text: '睡觉', completed: true },
    { text: '健身', completed: false },
  ],
  showing: 'COMPLETED'
}
````
这个对象就像是一个model(MVC中的model概念)，不过它没有setters  
这么做的原因是确保其他代码片段不能随意更改state值(这种做法会导致bug难以复现)
*** 
## action
````javascript
{ type: 'ADD_TODO', text: '学习React' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_SHOWING', filter: 'SHOW_ALL' }
````
想要改变state值，就只能dispatch an action  
action就是如上这样一个平淡无奇的JS对象，用以描述将发生的更改  
那么接下来reducers就根据action.type和payload来执行相应操作  
***
## reducers
````javascript
// reducer for 'SET_SHOWING'
function setShowing(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_SHOWING') {
    return action.filter
  } else {
    return state
  }
}
// reducer for 'ADD_TODO' and 'TOGGLE_TODO'
function handleChange(state = [], action) {
  switch(action.type) {
    case 'ADD_TODO':
      return state.concat([{text:action.text, completed: false}])
    case 'TOGGLE_TODO':
      return state.map((todo, index) => {
        action.index === index
        ? { text: todo.text, completed: !todo.completed }
        : todo
      })
    default:
      return state
  }
}
// reducer that manage the complete state
function handleTodo(state = {}, action) {
  return {
    todo_list: handleChange(state.todo_list, action),
    showing: setShowing(state.showing, action)
  }
}
````

***
[上一篇：包的安装](./2%20Installation.md)
<u style="float:right;">[下一篇：待定](./3%20Core%20Concepts.md)</u>