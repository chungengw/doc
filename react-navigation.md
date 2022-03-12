# react native路由

Version: 6.x

## 安装react navigation
```
yarn add @react-navigation/native @react-navigation/native-stack react-native-screens react-native-safe-area-context
```

## App.js
```jsx
import * as React from 'react'
import { View, Text } from 'react-native'

import { NavigationContainer } from '@react-navigation/native'
import { createNativeStackNavigator } from '@react-navigation/native-stack'

import Home from './Home'
import About from './About'

// Stack navigator实例，每个实例存储各自的导航状态
const Stack = createNativeStackNavigator()
const AnotherStack = createNativeStackNavigator()
const Tab = createBottomTabNavigator()

const isSignedIn = true
function App() {
  return (
    <NavigationContainer>
      {/*<Tab.Navigator>
        <Tab.Screen name="TabOne" component={TabOne} />
        <Tab.Screen name="TabTwo" component={TabTwo} />
      </Tab.Navigator>*/}

      <Stack.Navigator>
        isSignedIn ? (
          <>
            <Stack.Screen name="Home" component={Home} />
            <Stack.Screen name="About" component={About} />
          </>
        ): (
          <Stack.Screen name="SignIn" component={SignIn} />
        )
      </Stack.Navigator>

      {/*<AnotherStack.Navigator>
        <AnotherStack.Screen />
      </AnotherStack.Navigator>*/}
    </NavigationContainer>
  )
}

export default App
```

跳转到另一个页面：
```jsx
import React from 'react'
import { Text } from 'react-native'

function Home({ navigation }) {
  return (
    <>
      <Text onPress={() => navigation.navigate('About')}>go to About</Text>
      <Text onPress={() => navigation.goBack()}>go back</Text>
      <Text onPress={() => navigation.popToTop()}>go back to first screen in stack</Text>
      <Text onPress={() => navigation.push('About')}>push screen into stack always</Text>
    </>
  )
}

export default Home
```

## 传参

> 参数应该包含显示屏幕所需的最少数据，仅此而已

将参数传递给路由：
```jsx

function FooScreen(navigation) {
  return (
    <Button
      onPress={() => navigation.navigate('BarScreen', { query: 'FooScreen' })}
    ></Button>
  )
}

// 1.获取参数
function BarScreen({ route, navigation }) {
  const { query } = route.params
  return (
    <Button 
      // 2.更新路由参数
      onPress={() => navigation.setParams({ query: 'new params' })}
    ></Button>
  )
}
```

设置默认参数 initialParams：该参数将与自定义参数浅合并
```jsx
<Stack.Screen name="BarScreen" initialParams={{ itemId: 42 }} />
```

将参数传递到上一个页面：
```
TODO
```

将参数传递给嵌套页面：
```js
navigation.navigate('Account', {
  screen: 'Settings',
  params: { user: 'jane' },
});
```

## 配置

[完整配置](https://reactnavigation.org/docs/native-stack-navigator/#options)
```jsx
function StackScreen() {
  return (
    <Stack.Navigator
      // 公共配置
      screenOptions={{
        title: 'Overview',
        headerTitle: 'Overview' || (props) => (<LogoTitle {...props} />),
        headerBackVisible: false, // 是否显示返回按钮（stack（通过createNativeStackNavigator()创建）的第一个页面除外）
        headerShown: true, // 是否显示标题
        headerRight: () => (<Button title="Info" />)
      }}
    >
      <Stack.Screen
        name="Home"
        component={Home}
        // 私有配置
        options={{
          title: 'Overview',
        }}
      />
      <Stack.Screen
        name="Home"
        component={Home}
        // 支持function形式
        options={(route) => ({
          title: route.params.title || 'Overview'
        })}
      />
    <Stack.Navigator>
  )
}
```