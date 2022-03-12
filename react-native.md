## 关于Expo

- expo 基于 React Native 并整合了大量常用的 native module
- 因其整合了常用 native module ，如不增加额外的 native module 其可支持开发者本地无 Xcode 与 Android IDE 的情况下开发 react native app；
  也就是说只需要 nodejs 环境，非常便利- expo 的 mobile client 提供了非常便利的预览及测试环境（比 test flight 方便多了）
- 其快捷提供了非常酷的 OTA “热更新”功能 https://docs.expo.dev/versions/latest/guides/configuring-ota-updates
- 其云平台提供的 build 服务特方便
- 如果需要额外 native module 使用 expo 会有些不便
- expo 提供的 app push notification 无法满足国内情况，所以这一块比较麻烦
- 当前版本其默认在 App 首次启动时访问 expo 提供的 cdn 来获取 app 的静态资源，会导致离线情况下 App 启动卡住，
  而且国内访问其资源也有线路问题。不过：可以配置把资源 build 进 App https://docs.expo.dev/guides/configuring-updates/?redirected
- 一些配置问题还是要 build 后测试才会体现

参考：
- https://www.zhihu.com/question/61244146?sort=created
- https://www.jianshu.com/p/62462b2b0eb9


## SafeAreaView组件

> While React Native exports a SafeAreaView component, it has some inherent issues,
> i.e. if a screen containing safe area is animating, it causes jumpy behavior.
> In addition, this component only supports iOS 10+ with no support for older iOS versions or Android.
> We recommend to use the react-native-safe-area-context library to handle safe areas in a more reliable way.

推荐：
- [react-native-safe-area-context](https://github.com/th3rdwave/react-native-safe-area-context)

参考：
- https://reactnavigation.org/docs/handling-safe-area
