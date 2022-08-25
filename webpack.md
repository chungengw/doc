## webpack 5 移除了node核心模块

报错： "webpack < 5 used to include polyfills for node.js core modules by default. This is no longer the case. Verify if you need this module and configure a polyfill for it"

1. 安装
```
npm install node-polyfill-webpack-plugin
```

2. 需改 webpack 配置
```
const NodePolyfillPlugin = require('node-polyfill-webpack-plugin')

module.exports = {
  // ...
  plugins: [
    new NodePolyfillPlugin(),
  ],
}
```