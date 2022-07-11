

CommonJS - NodeJS中使用的模式

```js
// module-a.js
var data = "hello world";
function getData() {
  return data;
}
module.exports = {
  getData,
};

// index.js
const { getData } = require("./module-a.js");
console.log(getData());
```

问题:

1. 适用于服务端, 无法再前端页面直接使用,
2. 本身约定同步方式加载, 不适用于前端, 页面卡顿之类js 解析阻塞



AMD - requireJS 库

```js
// main.js
define(["./print"], function (printModule) {
  printModule.print("main");
});

// print.js
define(function () {
  return {
    print: function (msg) {
      console.log("print " + msg);
    },
  };
});

// module-a.js
require(["./print.js"], function (printModule) {
  printModule.print("module-a");
});

```



CMD - SeaJS 已被 requireJS 兼容



ES6 Module ESM

浏览器原生支持的加载模式, 在 node12.20 版本后已被支持

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/src/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="./main.js"></script>
  </body>
</html>
```

无需打包, 模块处理交给浏览器本身, 所以会很快

如果在 node 环境, 在 package.json 中声明 type:module 会默认使用 esm规范去解析

在 Node 的 commonJS模式里也可以加载 esm 模块

```js
async function func() {
  // 加载一个 ES 模块
  // 文件名后缀需要是 mjs
  const { a } = await import("./module-a.mjs");
  console.log(a);
}

func();

module.exports = {
  func,
};
```

