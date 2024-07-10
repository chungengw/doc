# uni-app: vue3

## `export default` 外的代码有2个注意事项

- 影响应用性能。这部分代码在应用启动时执行，而不是页面加载。
  如果这里的代码写的太复杂，会影响应用启动速度，占用更多内存。
- 不跟随组件、页面关闭而回收。在外层的静态变量不会跟随页面关闭而回收。
  如果必要你需要手动处理。比如 `beforeDestroy` 或 `destroyed` 生命周期进行处理。

## [nvue 开发与 vue 开发的常见区别](https://uniapp.dcloud.net.cn/tutorial/page.html)

- nvue 页面控制显隐只可以使用 `v-if` 不可以使用 `v-show`。
- nvue 页面的布局排列方向默认为竖排。
- flex 布局默认为垂直方向。
- nvue 切换横竖屏时可能导致样式出现问题，建议有 nvue 的页面锁定手机方向。
- css 选择器支持的比较少，只能使用 class 选择器。[详见](https://uniapp.dcloud.net.cn/tutorial/nvue-css)
- nvue 的各组件在安卓端默认是透明的，如果不设置 `background-color`，可能会导致出现重影的问题。
- class 进行绑定时只支持数组语法。
- Android 端在一个页面内使用大量圆角边框会造成性能问题，尤其是多个角的样式还不一样的话更耗费性能。应避免这类使用。
- 在 App.vue 中定义的全局 js 变量不会在 nvue 页面生效。`globalData` 和 `vuex` 是生效的。
- 不能在 style 中引入字体文件，nvue 中字体图标的使用参考：[加载自定义字体](https://uniapp.dcloud.net.cn/。tutorial/nvue-api#addrule)。如果是本地字体，可以用plus.io的 API 转换路径。
- 目前不支持在 nvue 页面使用 typescript/ts。
- **强烈建议**在 nvue 页面使用原生导航栏。

## 模板内引入静态资源

- 支付宝小程序组件内 image 标签不可使用相对路径。
- pages 下的静态资源需要在js中用 `import`引入。

## css语法

单位转换：

- 设计稿 1px / 设计稿基准宽度 = 框架样式 1rpx / 750rpx，即 css 尺寸(rpx) = 设计稿尺寸(px) * 750 / 设计稿基准宽度(px)。
  举例：
  1. 若设计稿宽度为 750px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 * 100 / 750，结果为：100rpx。
  2. 若设计稿宽度为 640px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 * 100 / 640，结果为：117rpx。
  3. 若设计稿宽度为 375px，元素 B 在设计稿上的宽度为 200px，那么元素 B 在 uni-app 里面的宽度应该设为：750 * 200 / 375，结果为：400rpx。
- rpx 不支持动态横竖屏切换计算，使用 rpx 建议锁定屏幕方向。

选择器：

- **微信小程序自定义组件中仅支持 class 选择器**。
- 目前支持的选择器有：
  - `.class`
  - `#id`
  - `element`
  - `element, element`
  - `::after` 仅 vue 页面生效
  - `::before` 仅 vue 页面生效
- `page` 选择器相当于 `body` 节点，设置页面背景颜色，使用 `<style scoped>` 会导致失效。

[背景图片：](https://uniapp.dcloud.net.cn/tutorial/syntax-css.html#%E8%83%8C%E6%99%AF%E5%9B%BE%E7%89%87)
uni-app 支持使用在 css 里设置背景图片，使用方式与普通 web 项目大体相同，但需要注意以下几点：

- 小程序不支持在 css 中使用本地文件，包括本地的背景图和字体文件。需以 base64 方式方可使用。
- 使用本地路径背景图片需注意：
  1. 为方便开发者，在背景图片小于 40kb 时，uni-app 编译到不支持本地背景图的平台时，会自动将其转化为 base64 格式。
  2. 图片大于等于 40kb，会有性能问题，不建议使用太大的背景图。
  3. 本地背景图片的引用路径推荐使用以 ~@ 开头的绝对路径，`background-image: url('~@/static/logo.png');`。

## Vue3

- 建议开发过程中直接使用 uni-app：[表单组件](https://uniapp.dcloud.io/component/button)。

- 事件修饰符:
  - `.stop`: 各平台均支持， 使用时会阻止事件冒泡，在非 H5 端同时也会阻止事件的默认行为
  - `.prevent`: 仅在 H5 平台支持
  - `.capture`: 仅在 H5 平台支持
  - `.self`: 仅在 H5 平台支持
  - `.once`: 仅在 H5 平台支持
  - `.passive`: 仅在 H5 平台支持
- `uni-app x` 只支持 `stop` 和 `once`。
- 若需要禁止蒙版下的页面滚动，可使用 `@touchmove.stop.prevent="moveHandle"`，
  `moveHandle` 可以用来处理 `touchmove` 的事件，也可以是一个空函数。
- uni-app 运行在手机端，没有键盘事件，所以不支持按键修饰符。

事件映射表：

```js
// 事件映射表，左侧为 WEB 事件，右侧为 ``uni-app`` 对应事件
{
  click: 'tap',
  touchstart: 'touchstart',
  touchmove: 'touchmove',
  touchcancel: 'touchcancel',
  touchend: 'touchend',
  tap: 'tap',
  longtap: 'longtap', //推荐使用longpress代替
  input: 'input',
  change: 'change',
  submit: 'submit',
  blur: 'blur',
  focus: 'focus',
  reset: 'reset',
  confirm: 'confirm',
  columnchange: 'columnchange',
  linechange: 'linechange',
  error: 'error',
  scrolltoupper: 'scrolltoupper',
  scrolltolower: 'scrolltolower',
  scroll: 'scroll'
}
```

## UTS

[类型兼容：](https://doc.dcloud.net.cn/uni-app-x/uts/type-compatibility.html#import-type)

```js
import type { TypeA } from './type' // 避免使用
import { TypeA } from './type' // 推荐用法
```

## 编译器（条件编译）

- **VUE3 需要在项目的 `manifest.json` 文件根节点配置 `"vueVersion" : "3"`。**