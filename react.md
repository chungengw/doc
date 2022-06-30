## lazy() 与热更新

路由中使用lazy()方式加载组件时，若该组件到处了多个对象会导致热更新失效，复现

```
// MyComponent.js
export ...
export default ...

// routes.js
const MyComponent = lazy(() => import('./MyComponent))
```

解决方法：临时改变 MyComponent 导入方式为
```
// routes.js
import MyComponent from './MyComponent'
```