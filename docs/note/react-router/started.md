# 简介

React Router 是一个基于 React 之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步。

React Router 知道如何为我们搭建嵌套的 UI，因此我们不用手动找出需要渲染哪些 <Child> 组件。举个例子，对于一个完整的 /about 路径，React Router 会搭建出 `<App><About /></App>`。

```
this.props.params
this.props.location.query
```

应用的 UI 以盒子中嵌套盒子的方式来表现；然后你可以让这些盒子与 URL 始终保持同步，而且很容易地把它们链接起来。
