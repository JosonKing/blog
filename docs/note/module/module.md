# JS 模块化规范

> 原始方式

- 模块的引入顺序可能会出错
- 会污染全局变量
- 模块之间的依赖关系不明显

> 主流规范

- CommonJS
- AMD
- CMD
- ES6 Module

## CommonJS

    一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，模块必须通过 module.exports 导出对外的变量或接口，通过 require() 来导入其他模块的输出到当前模块作用域中。

> NodeJs 中 CommonJS 的模块化机制

```
// 模块定义 add.js
module.eports.add = function(a, b) {
  return a + b;
};
```

```
// 模块定义 decrease.js
module.exports.decrease = function(a, b) {
  return a - b;
};
```

```
// formula.js,模块使用，利用 require() 方法加载模块,require 导出的即是 module.exports 的内容
const add = require("./add.js").add;
const decrease = require("./decrease.js").decrease;
module.exports.square_difference = function(a, b) {
  return add(a, b) * decrease(a, b);
};
```

> module.exports = exports = {}

## AMD(Asynchronous Module Definition)

    异步模块加载机制，它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句都定义在一个回调函数中，等到依赖加载完成之后，这个回调函数才会运行。

- 实现 JavaScript 文件的异步加载，避免网页失去响应
- 管理模块之间的依赖性，便于代码的编写和维护

## CMD (Common Module Definition)

    CMD 在浏览器端的实现有 SeaJS， 和 RequireJS 一样，SeaJS 加载原理也是动态创建异步 Script 标签。二者的区别主要是依赖写法上不同，AMD 推崇一开始就加载所有的依赖，而 CMD 则推崇在需要用的地方才进行依赖加载。

## ES Module

    ECMAScript 6 中引入的模块化功能。模块功能主要由两个命令构成，分别是 export 和 import。export 命令用于规定模块的对外接口，import 命令用于输入其他模块提供的功能。

```
// 模块定义 add.js
export function add(a, b) {
  return a + b;
}

// 模块使用 main.js
import { add } from "./add.js";
console.log(add(1, 2)); // 3
```

> export 和 export default

    在一个文件或模块中，export 可以有多个，export default 仅有一个, export 类似于具名导出，而 default 类似于导出一个变量名为 default 的变量。同时在 import 的时候，对于 export 的变量，必须要用具名的对象去承接，而对于 default，则可以任意指定变量名

```
// a.js
export var a = 2;
export var b = 3 ;
// main.js 在导出的时候必须要用具名变量 a, b 且以解构的方式得到导出变量
import {a, b} from 'a.js' // √ a= 2, b = 3
import a from 'a.js' // x

// b.js export default 方式
const a = 3
export default a // 注意不能 export default const a = 3 ,因为这里 default 就相当于一个变量名

// 导出
import b form 'b.js' // √
import c form 'b.js' // √ 因为 b 模块导出的是 default，对于导出的default，可以用任意变量去承接
```

> ES Module 模块加载和导出过程

```
// counter.js
export let count = 5

// display.js
export function render() {
  console.log('render')
}

// main.js
import { counter } from './counter.js';
import { render } from './display.js'
```
