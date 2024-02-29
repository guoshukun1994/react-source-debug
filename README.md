# React v18.2.0 源码调试环境

本仓库基于 create-react-app 创建。

React v18.2.0 源代码存放于 `src/react` 目录，作为 git submodules 的方式组织。

### 安装方法：

```sh
git clone --recurse-submodules git@github.com:camsong/react-source-debug.git
cd react-source-debug
cd src
git clone https://github.com/facebook/react.git
cd ..
yarn install
yarn start
```

### 修改项目配置

1. config/webpack.config.js
   搜索 alias

```js
alias: {
        react: path.resolve(__dirname, "../src/react/packages/react"),
        "react-dom": path.resolve(__dirname, "../src/react/packages/react-dom"),
        "react-reconciler": path.resolve(__dirname, "../src/react/packages/react-reconciler"),
        "react-dom-bindings": path.resolve(__dirname, "../src/react/packages/react-dom-bindings"),
        scheduler: path.resolve(__dirname, "../src/react/packages/scheduler"),
        shared: path.resolve(__dirname, "../src/react/packages/shared"),
      },
```
2. 修改 env.js 文件 配置 webpack DefinePlugin 的环境参数
   **搜索 stringified**
   "__DEV__": true,
    "__PROFILE__": true,
    "__UMD__": true,
    "__EXPERIMENTAL__": true
3. 启动项目，处理报错
   1. 解决 src/index.js 下的导出错误
    import * as React from 'react';
    mport * as ReactDOM from 'react-dom/client';
   2. 处理 react-reconciler/src/Scheduler.js 编译错误
    两个mock变量 unstable_yieldValue、unstable_setDisableYieldValue 赋值空函数()=>{}
4. 添加 .env 文件
   **禁用eslint**
   DISABLE_ESLINT_PLUGIN=true
5. 解决 ReactSharedInternals 的报错
   import ReactSharedInternals from '../react/src/ReactSharedInternals'
6. 解决 ReactFiberHostConfig 的报错
   这个文件其他内容是rollup动态注入的，决定渲染终端的方式
   删除抛出异常的代码，添加代码`export * from './forks/ReactFiberHostConfig.dom'`
7. 如果有因为 flow 类型导致的 ts 报错，可以直接关掉 ts 报错
   settings -> 搜索 script:validate -> 把出现的 JavaScript 和 TypeScript 的 validation 取消勾选


   

   
    
   
    
   

### 开始调试

打开 http://localhost:3000/ 即可开始调试。
建议先完成以下任务，增加对 React 了解：

1. 火焰图查看代码的调用堆栈，并跳转到感兴趣的代码
1. 查看 workloop 的代码 `src/react/packages/react-reconciler/src/ReactFiberWorkLoop.old.js`
1. 查看 render(beginWork, completeWork)、commit 阶段
1. 查看 Fiber 的数据结构 `src/react/packages/react-reconciler/src/ReactInternalTypes.js`
1. 查看 Hooks 的数据结构 `src/react/packages/react-reconciler/src/ReactFiberHooks.old.js`
