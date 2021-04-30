# 高级指引

## 代码分割

### 打包

大多数 React 应用都会使用 Webpack，Rollup 或 Browserify 这类的构建工具来打包文件。 打包是一个将文件引入并合并到一个单独文件的过程，最终形成一个 “bundle”。 接着在页面上引入该 bundle，整个应用即可一次性加载。

示例
App 文件：

```
// app.js
import { add } from './math.js';
```

```
console.log(add(16, 26)); // 42
// math.js
export function add(a, b) {
  return a + b;
}
```

打包后文件：

```
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42
```

### 代码分割

打包是个非常棒的技术，但随着你的应用增长，你的代码包也将随之增长。尤其是在整合了体积巨大的第三方库的情况下。你需要关注你代码包中所包含的代码，以避免因体积过大而导致加载时间过长。

为了避免搞出大体积的代码包，在前期就思考该问题并对代码包进行分割是个不错的选择。 代码分割是由诸如 Webpack，Rollup 和 Browserify（factor-bundle）这类打包器支持的一项技术，能够创建多个包并在运行时动态加载。

对你的应用进行代码分割能够帮助你“懒加载”当前用户所需要的内容，能够显著地提高你的应用性能。尽管并没有减少应用整体的代码体积，但你可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。

#### import()

在你的应用中引入代码分割的最佳方式是通过动态 import() 语法。

使用之前：

```
import { add } from './math';

console.log(add(16, 26));
```

使用之后：

```
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

当 Webpack 解析到该语法时，会自动进行代码分割。如果你使用 Create React App，该功能已开箱即用，你可以立刻使用该特性。Next.js 也已支持该特性而无需进行配置。

如果你自己配置 Webpack，你可能要阅读下 Webpack 关于代码分割的指南。你的 Webpack 配置应该类似于此。

当使用 Babel 时，你要确保 Babel 能够解析动态 import 语法而不是将其进行转换。对于这一要求你需要 babel-plugin-syntax-dynamic-import 插件。

## Context

> Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。

在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。

### 何时使用 Context

Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

### 使用 Context 之前的考虑

在组件树中很多不同层级的组件需要访问同样的一批数据。Context 能让你将这些数据向组件树下所有的组件进行“广播”，所有的组件都能访问到这些数据，也能访问到后续的数据更新。使用 context 的通用的场景包括管理当前的 locale，theme，或者一些缓存数据，这比替代方案要简单的多。

### API

## 错误边界

### 错误边界（Error Boundaries）

部分 UI 的 JavaScript 错误不应该导致整个应用崩溃，为了解决这个问题，React 16 引入了一个新的概念 —— 错误边界。

错误边界是一种 React 组件，这种组件可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

> 错误边界无法捕获以下场景中产生的错误：
>
> - 事件处理（了解更多）
> - 异步代码（例如 setTimeout 或 requestAnimationFrame 回调函数）
> - 服务端渲染
> - 它自身抛出来的错误（并非它的子组件）

如果一个 class 组件中定义了 static getDerivedStateFromError() 或 componentDidCatch() 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。当抛出错误后，请使用 static getDerivedStateFromError() 渲染备用 UI ，使用 componentDidCatch() 打印错误信息。

错误边界的工作方式类似于 JavaScript 的 catch {}，不同的地方在于错误边界只针对 React 组件。只有 class 组件才可以成为错误边界组件。大多数情况下, 你只需要声明一次错误边界组件, 并在整个应用中使用它。

注意错误边界仅可以捕获其子组件的错误，它无法捕获其自身的错误。如果一个错误边界无法渲染错误信息，则错误会冒泡至最近的上层错误边界，这也类似于 JavaScript 中 catch {} 的工作机制。

### 未捕获错误（Uncaught Errors）的新行为

自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载。

## Refs 转发

> Ref 转发是一项将 ref 自动地通过组件传递到其一子组件的技巧。对于可重用的组件库是很有用的。

### 转发 refs 到 DOM 组件

FancyButton 或 MyTextInput 这样的高可复用“叶”组件倾向于在整个应用中以一种类似常规 DOM button 和 input 的方式被使用，并且访问其 DOM 节点对管理焦点，选中或动画来说是不可避免的。

Ref 转发是一个可选特性，其允许某些组件接收 ref，并将其向下传递（换句话说，“转发”它）给子组件。

## 高阶组件

> 高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

> 高阶组件是参数为组件，返回值为新组件的函数。

### 使用 HOC 解决横切关注点问题

在一个大型应用程序中，我们需要一个抽象，允许我们在一个地方定义相同逻辑，并在许多组件之间共享它。这正是高阶组件擅长的地方。

编写一个创建组件函数。该函数将接受一个子组件作为它的其中一个参数，该子组件将订阅数据作为 prop。

HOC 不会修改传入的组件，也不会使用继承来复制其行为。相反，HOC 通过将组件包装在容器组件中来组成新组件。HOC 是纯函数，没有副作用。

被包装组件接收来自容器组件的所有 prop，同时也接收一个新的用于 render 的 data prop。HOC 不需要关心数据的使用方式或原因，而被包装组件也不需要关心数据是怎么来的。

### 不要改变原始组件,使用组合

## 与第三方库协同

### 集成带有 DOM 操作的插件

我们会添加一个 ref 到这个根 DOM 元素。 在 componentDidMount 中，我们能够获取它的引用这样我们就可以把它传递给 jQuery 插件了。

为了防止 React 在挂载之后去触碰这个 DOM，我们会从 render() 函数返回一个空的 `<div />`。这个 `<div />` 元素既没有属性也没有子元素，所以 React 没有理由去更新它，使得 jQuery 插件可以自由的管理这部分的 DOM：

## 深入 JSX

JSX 仅仅只是 React.createElement(component, props, ...children) 函数的语法糖。如下 JSX 代码：

```
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

会编译为：

```
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```

### 在运行时选择类型

```
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 正确！JSX 类型可以是大写字母开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```
