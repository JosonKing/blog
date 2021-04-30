# React/Vue 添加`<script>`标签

## 基本步骤

1. 创建`<script>标签`

```JavaScript
let script = document.createElement('script');
```

2. 为`<script>标签`添加属性

```JavaScript
// W3C标准属性
script.type = 'text/javascript';
script.src = './jquery.js';

// W3C未规定的自定义属性
script.setAttribute('FOR','domId'); // 监听目标dom
script.setAttribute('EVENT',"InitializationCompleted()"); // 监听消息
```

3. 将`<script>标签`添加到`html`上

```JavaScript
document.body.appendChild(script);
```

最终页面生成内容：

```JavaScript
<script type="text/javascript" src="./jquery.js"></script>
```

## React 添加

```JavaScript
componentWillMount() {
  let script = document.createElement('script');
  script.type = 'text/javascript';
  script.src = './jquery.js';
  document.body.appendChild(script);
}
```

## Vue 添加

```JavaScript
created() {
  let script = document.createElement('script');
  script.id = 'myScript';
  script.type = 'text/javascript'

  // 以下是监听指定dom的消息，domId为监听的dom的id，不需要可忽略
  script.setAttribute('FOR','domId');
  script.setAttribute('EVENT',"InitializationCompleted()");
  let node = document.createTextNode(`
    console.log('InitializationCompleted');
    _initDevice();
  `);
  script.appendChild(node);

  document.body.appendChild(hspScript);
}

mounted() {
  // 以下是设置全局方法，为了在监听dom消息后执行，不需要可忽略
  window['_initDevice'] = this.initDevice;
}
```
