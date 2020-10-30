# 介绍
Redux是一个为JS应用而生状态控制容器  
Redux is a predictable state container for Javascript apps  
# 优势
能帮助你开发的应用在不同环境下(client, server or native)保持行为一致性  
提供优秀的开发体验， 可以实时编辑，时空穿梭debug  
既可以选择与React联用，也可以与其他视图库联用  
代码体积小，且可选插件多(生态好)  
# 工具包
RTK(React Toolkit): 官方推荐编写Redux逻辑的工具包  
RTK围绕Redux核心，包含创建Redux App常用的包和函数  
内置建议的最佳实践，简化大多数Redux task，避免常见错误  
简化你的store setup, reducers creation, 编写immutable logic，甚至创建整个state的slice  
<u>RTK适用于初学者，也适用与有经验的开发人员</u>
# 理解
当你适用了Redux作为状态容器来管理App状态时：  
- app的全部state都存储在一个对象树里（object tree inside a single store）
- 改变state树的唯一办法是：emit an 'action'(一个描述发生什么的对象)
- Redux根据action执行更改(执行更改的函数叫做reducers)

一个典型(小型)的Redux app只有一个store和一个根reducer function  
当系统庞大时，root reducer分割成多个reducer来分别作用于不同的state tree  
正是这个特质使得Redux能够处理大型复杂系统，也使得时空穿梭debug成为可能（因为它记录了每个触发改变的action）

# 你可能不需要Redux
不要使用Redux仅仅因为别人说你应该用
- 你有大量随时间改变的数据时
- 你需要唯一数据源时
- 你发现把所有state放到顶级组件里已经不能满足要求时

你才应该使用Redux！