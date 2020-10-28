# 状态提升
- lifting child's local state up to its parent's local state
- 当组件A与组件B共同使用相同数据, 数据存储在A与B的state中时 
- 可以将共用数据放到其最近的父元素C的state中去, 让C通过props传递给A与B
- 这样, C的state是A与B的唯一数据源, 就保证了数据的一致性
***
示例程序(温度转换)见: <u>**myapp>Calculator**</u>