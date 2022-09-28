---
title: 面试笔记
date: 2020/10/25
tags: [typescript]
categories: 
- typescript
series: typescript
---
## webpack
### 基础
#### entry
#### output
#### loader
#### plugin
#### babel
### 开发阶段优化
#### dllplugin 第三方包缓存
[参考](http://www.ddpool.cn/article/42654.html)
#### css sourcemap根据需要打开
#### 路由的层面做懒加载react-loadable
#### 热重载与热替换

### 生产阶段优化
#### 长期资源通过hash值做缓存
静态文件缓存方案：这个最常看到。现在流行的方式是文件hash+强缓存的一个方案。比如hash+ cache control: max-age=1年。
#### 代码压缩
#### 减少http请求（图片的base64）
### 首屏优化
[这篇文章讲得好](https://juejin.im/post/6844904009581461518)
- 服务端渲染ssr next.js
- 服务器端进行gzip 传输文件压缩
- 组建按需加载
- cdn分发
通过在多台服务器部署相同的副本，当用户访问时，服务器根据用户跟哪台服务器地理距离小或者哪台服务器此时的压力小，来决定哪台服务器去响应这个请求。
- 缓存

## 其他
### http2与http1区别
- 二进制传输
http2采用二级制传输，相对于http1的文本传输安全性要高
- 多路复用
http一个链接只能提交一个请求，而http2能同时处理无数个请求，可以降低连接的数量，提高网络的吞吐量。
- 头部压缩
http2通过gzip与compress对头部进行压缩，并且在客户端与服务端各维护了一份头部索引表，只需要根据索引id就可以进行头部信息的传输，缩小了头部容量，间接提升了传输效率。
- 服务端推送
http2可以主动推送资源到客户端，避免客户端花过多时间逐个请求，降低相应时间

### mvc mvvm 与 react vue
mvc= model 数据 + view 视图 + controler 逻辑处理／数据调度者(单向)
mvvm = model 数据 + view 视图 + viewmodel 调度者（双向绑定）
[参考](https://www.jianshu.com/p/220729f01a25)
[参考](https://www.bilibili.com/video/BV1Xf4y1m7x5?from=search&seid=14281577773722133844)
[双向绑定](https://segmentfault.com/a/1190000006599500)
mvc：
- View 传送指令到 Controller
- Controller 完成业务逻辑后，要求 Model 改变状态
- Model 将新的数据发送到 View，用户得到反馈
所有通信都是单向的。
mvc的主要特点是单向数据流，

mvvm：
- View的变动，自动反映在 ViewModel，反之亦然。
- viewmodel 将数据与 model同步
- model 与 view 不直接联系
mvvm主要特点是双向绑定，model与view不直接交互，而是通过vm。

目前几种主流的mvc(vm)框架都实现了单向数据绑定，而我所理解的双向数据绑定无非就是在单向绑定的基础上给可输入元素（input、textare等）添加了change(input)事件，来动态修改model和 view，并没有多高深。所以无需太过介怀是实现的单向或双向绑定。
数据劫持（vue.js）

model 数据改变 就通过vm(调度器) 让view层响应数据的渲染；
view层 渲染改变的时候，通过vm(给组件绑定事件)， 让model数据跟着改变；


react 应该是一个 model+view 的 mv框架，加上redux变成 mvc。
mvc除了在前端，其实最早是在后端运用
另外一个图片参考：
![](/image/git/mvvm.png)

### redux redux-thunk react-redux
#### redux 概述
![](/image/end/redux.jpg)
#### API
- createStore(reducers)
- store.getState
- store.dispatch
- store.subscribe
- store.subscribe
- combineReducers ：Reducer 的拆分
#### Reducer 的拆分
使用：`import { combineReducers } from 'redux';`
#### 参考
[redux](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)
### 为什么废弃react生命周期函数
[为什么废弃react生命周期函数](https://segmentfault.com/a/1190000021272657)
废弃的原因主要两个
- 是因为fiber的出现，很可能因为高优先级任务的出现而打断现有任务导致它们会被执行多次
- 让开发能够更加规范和优雅写代码 如 componentWillReceiveProps
具体如下：
- componentWillMount
 - 在Fiber之后， 由于任务可中断，willMount可能会被执行多次
 - 完全可以将之前功能写到 constructor 和 componentDidmount 上
- componentWillUpdate
 - 在Fiber之后， 由于任务可中断，willMount可能会被执行多次
 - 可以将功能移到componentDidUpdate
- componentWillReceiveProps
 - 主要做props来改变state，这样写导致state值来源不单一或看代码复杂，比如你还可以在componentWillReceiveProps上引用class其他内部方法，在其他内部方法内又可以做setState，，，新的做法将这个功能独立到 静态函数 getDriveStatefromProps，就很单纯而清晰。
 - 另外要处理的一些非state逻辑，可以移到 componentDidUpdate

 ### 一个对象继承另外一个对象(Object.defineProperty)
 下面实现了 dd 继承了 cc，当cc改变时，dd也会跟着变。
 其实就是用了数据劫持 Object.defineProperty
 ```js
    var dd = {a:1,b:5};
    var cc = {fn:(e)=>{console.log(e||111)}, aq:123};
    Object.entries(cc).forEach(([key, value]) => {
        Object.defineProperty(dd, key, {
            enumerable:false,
            configurable:false,
            get:()=>{
                return cc[key];
            }
        })
    });
 ```
 ### 性能优化是重中之重
 要专研这方面的很多情况，设想很多情况下的，很多情景下的问答

 ### 设计模式
 设计模式需要复习

 ### ngnix代理
 #### 概述
 node java 用于后端写业务；
 ngnix 后端代理转发，好处有：
 - 不用修改原来比如node java 写的代码，做一些升级 如将服务升级到http2；
 - 将一些纯后端配置优化的东西内聚，让node java 专注于写后端业务代码， 这些后端配置优化包含 代理缓存 http2 升级；
 
 #### ngnix代理缓存与服务端缓存
 服务端缓存 基于 浏览器，如果换一个浏览器请求需重新缓存；
 ngnix代理缓存 基于 用户， 如果用户第一次请求后，用户下次换各种不同浏览器 都可以享受缓存；
 #### 参考
 [HTTP协议原理+实践 Web开发工程师必学]()

 ### css3
 - transform-origin
 - animation ： animation:circle 2s infinite linear;
 - @keyframes circle
 - transition  :transition: width/all 0.4s ease-out;
 - box-shadow  :5px 5px blue
 - outline
 - background-image: linear-gradient(blue, green 30%, red 50%);（线性渐变）
 - background-position
 - @font-face
 - background-size: 100% auto 背景图片的宽度为元素宽度100%，高度由浏览器自动计算一个值，保持不失真(推荐)
 - background-origin 是否从border开始，是否从padding开始， 是否从content开始/background-clip

 ### promise
 最好手写一份promise，对面试也好，自己工作也好，也是非常好的

 ### 事件循环 event loop

 ### cookie 与 locastoreage 区别
 前者是给后端用的，后者是前端用

 ### 算法，如快速排序，还有递归

 ### xss 与 csrf 安全

 ### react diff算法

 ### node
 了解node 的express或koa，用node写过一些后台。

 ### 前端跨域解决方式

 ### 正则
























