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

const Stack = createNativeStackNavigator()

function App() {
  return (
    // NavigationContainer外层不能有任何其他容器
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={Home} />
        <Stack.Screen name="About" component={About} />
      </Stack.Navigator>
    </NavigationContainer>
  )
}

export default App
```

```jsx
import React from 'react'
import { Text } from 'react-native'

function Home({ navigation }) {
  return (
    <Text onPress={() => navigation.navigate('About')}>go to About</Text>
  )
}

export default Home
```