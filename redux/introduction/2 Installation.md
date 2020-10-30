# 安装RTK: 
````shell
# NPM
npm install @reduxjs/toolkit

# Yarn
yarn add @reduxjs/toolkit
````
RTK同样提供UMD build，全局变量: window.RTK
***
# 安装Redux Core: 
````shell
# NPM
npm install redux

# Yarn
yarn add redux
````
支持Webpack，Browserify，Roolup进行模块化引入  
同样提供UMD build，全局变量: window.Redux
***
# 补充包
Complementary Packages mean:   
react-redux (for react binding) and redux-devtools (the developer tools)
***
# 创建React Redux示例
````shell
npx create-react-app redux-app --template redux
````