## webpack 5 移除了node核心模块

报错： "webpack < 5 used to include polyfills for node.js core modules by default. This is no longer the case. Verify if you need this module and configure a polyfill for it"

1. 安装
```
npm install node-polyfill-webpack-plugin
```

2. 需改 webpack 配置
```js
const NodePolyfillPlugin = require('node-polyfill-webpack-plugin')

module.exports = {
  // ...
  plugins: [
    new NodePolyfillPlugin(),
  ],
}
```

------------------

1. `npm install ...`

2.webpack 配置
```js
module.exports = {
  configure(config) {
    const fallback = config.resolve.fallback || {};
    Object.assign(fallback, {
      crypto: require.resolve('crypto-browserify'),
      stream: require.resolve('stream-browserify'),
      assert: require.resolve('assert'),
      http: require.resolve('stream-http'),
      https: require.resolve('https-browserify'),
      os: require.resolve('os-browserify'),
      url: require.resolve('url'),
      path: require.resolve('path-browserify')
    })
    config.resolve.fallback = fallback;
    config.plugins = (config.plugins || []).concat([
      new webpack.ProvidePlugin({
        process: 'process/browser',
        Buffer: ['buffer', 'Buffer']
      })
    ])
  }
}
```

------------------

关闭polyfills模块

```js
module.exports = {
  resolve: {
    alias: {
      crypto: false,
      // ...
    }
  }
}
```