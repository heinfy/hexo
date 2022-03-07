---
title: RN基础入门
categories:
  - ReactNative
tags:
  - ReactNative
abbrlink: e61ae078
date: 2020-03-31 16:14:14
---

<!-- more -->

# 入门基础

## 核心组件和原生组件

### 原生组件

使用 React Native，您可以使用 React 组件通过 JavaScript 调用 Android 和 IOS 视图。 在运行时，React Native 为这些组件创建相应的 Android 和 iOS 视图。 由于 React Native 组件具有与 Android 和 iOS 相同的视图支持，因此 React Native 应用的外观，感觉和性能与其他任何应用一样。 我们将这些平台支持的组件称为“本机组件”。

通过 React Native，您可以为 Android 和 iOS 构建自己的 Native Components，以满足您应用的独特需求。 我们还有一个由社区贡献的组成部分的繁荣生态系统。 查看本机目录以查找社区正在创建的内容。

React Native 还包括一组基本的，随时可用的 Native 组件，您可以使用它们立即开始构建您的应用程序。 这些是 React Native 的核心组件。

### 核心组件

React Native 具有许多核心组件，从表单控件到活动指示器，应有尽有。 您可以在 API 部分找到所有记录的文档。 您将主要使用以下核心组件：

| RN UI 组件     | ANDROID VIEW   | IOS VIEW         | WEB ANALOG               | 说明                                                      |
| :------------- | :------------- | :--------------- | :----------------------- | :-------------------------------------------------------- |
| `<View>`       | `<ViewGroup>`  | `<UIView>`       | A non-scrollling `<div>` | 支持 flexbox 布局、样式、一些触摸处理和可访问性控制的容器 |
| `<Text>`       | `<TextView>`   | `<UITextView>`   | `<p>`                    | 显示，设置样式和嵌套文本字符串，甚至处理触摸事件          |
| `<Image>`      | `<ImageView>`  | `<UIImageView>`  | `<img>`                  | 显示不同类型的图像                                        |
| `<ScrollView>` | `<ScrollView>` | `<UIScrollView>` | `<div>`                  | 通用滚动容器，可以包含多个组件和视图                      |
| `<TextInput>`  | `<EditText>`   | `<UITextField>`  | `<input type="text">`    | 允许用户输入文字                                          |

```jsx
import React, { useState } from 'react';
import { View, Text, Image, ScrollView, TextInput, FlatList } from 'react-native';

const App = () => {
  return (
    <ScrollView>
      <Text>Some text</Text>
      <View>
        <Text>Some moqqqre text</Text>
        <Image source={{ uri: 'bd' }} style={{ width: 200, height: 200 }}></Image>
        <Image source={require('./1.png')} style={{ width: 200, height: 200, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={{ uri: 'https://facebook.github.io/react-native/docs/assets/p_cat2.png' }} style={{ width: 200, height: 200 }} />
      </View>
      <TextInput
        style={{
          height: 40,
          borderColor: 'gray',
          borderWidth: 1
        }}
        defaultValue='You can type in me'
      />
    </ScrollView>
  );
};

export default App;
```

## React 基础

### 组件

#### Function Component

```jsx
import React from 'react';
import { Text } from 'react-native';
export default function Cat() {
  return <Text>Hello, I am your cat!</Text>;
}
```

#### Class Component

```jsx
import React, { Component } from 'react';
import { Text } from 'react-native';
export default class Cat extends Component {
  render() {
    return <Text>Hello, I am your cat!</Text>;
  }
}
```

这两种不同的组件之间的**本质区别就是**：有无 state 属性 和 生命周期函数！

### JSX

```jsx
import React from 'react';
import { Text } from 'react-native';
export default function Cat() {
  function getFullName(firstName, secondName, thirdName) {
    return firstName + ' ' + secondName + ' ' + thirdName;
  }
  return <Text>Hello, I am {getFullName('Rum', 'Tum', 'Tugger')}!</Text>;
}
```

### 自定义组件

```jsx
import React, { Component } from 'react';
import { Button, Text, View } from 'react-native';
export class Cat extends Component {
  state = { isHungry: true };
  render(props) {
    return (
      <View>
        <Text>
          I am {this.props.name}, and I am
          {this.state.isHungry ? ' hungry' : ' full'}!
        </Text>
        <Button
          onPress={() => {
            this.setState({ isHungry: false });
          }}
          disabled={!this.state.isHungry}
          title={this.state.isHungry ? 'Pour me some milk, please!' : 'Thank you!'}
        />
      </View>
    );
  }
}
export default class Cafe extends Component {
  render() {
    return (
      <>
        <Cat name='Munkustrap' />
        <Cat name='Spot' />
      </>
    );
  }
}
```

### props

**类似于 vue 的父子组件传值**

```jsx
import React from 'react';
import { Text, View } from 'react-native';
function Cat(props) {
  return (
    <View>
      <Text>Hello, I am {props.name}!</Text>
    </View>
  );
}
export default function Cafe() {
  return (
    <View>
      <Cat name='Maru' />
      <Cat name='Jellylorum' />
      <Cat name='Spot' />
    </View>
  );
}
```

### state

**类似于 vue data 对象**

`useState `：

`[<getter>, <setter>] = useState(<initialValue>)`。

在函数组件中调用`useState`，就会创建一个单独的状态。

在类组件中，`state` 总是一个对象，可以在该对象上添加保存属性。

每次调用`useState`都会创建一个`state`块，其中包含一个值。

```jsx
import React, { useState } from 'react';
import { Button, Text, View } from 'react-native';
function Cat(props) {
  // 创建一个状态，并将其初始化为“true”
  const [isHungry, setIsHungry] = useState(true);
  return (
    <View>
      <Text>
        I am {props.name}, and I am {isHungry ? 'hungry' : 'full'}!
      </Text>
      <Button
        onPress={() => {
          setIsHungry(false);
        }}
        disabled={!isHungry}
        title={isHungry ? 'Pour me some milk, please!' : 'Thank you!'}
      />
    </View>
  );
}
export default function Cafe() {
  return (
    <>
      <Cat name='Munkustrap' />
      <Cat name='Spot' />
    </>
  );
}
```

```jsx
import React, { Component } from 'react';
import { Button, Text, View } from 'react-native';
export class Cat extends Component {
  // 声明状态
  state = { isHungry: true };
  render(props) {
    return (
      <View>
        <Text>
          I am {this.props.name}, and I am
          {this.state.isHungry ? ' hungry' : ' full'}!
        </Text>
        <Button
          onPress={() => {
            {
              /* 通过 setState 改变状态 */
            }
            this.setState({ isHungry: false });
          }}
          disabled={!this.state.isHungry}
          title={this.state.isHungry ? 'Pour me some milk, please!' : 'Thank you!'}
        />
      </View>
    );
  }
}
export default class Cafe extends Component {
  render() {
    return (
      <>
        <Cat name='Munkustrap' />
        <Cat name='Spot' />
      </>
    );
  }
}
```

## RN 的 Props

### 原生组件的 props

以常见的基础组件`Image`为例，在创建一个图片时，可以传入一个名为`source`的 prop 来指定要显示的图片的地址，以及使用名为`style`的 prop 来控制其尺寸。

```jsx
import React, { Component } from 'react';
import { Image } from 'react-native';

export default class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return <Image source={pic} style={{ width: 193, height: 110 }} />;
  }
}
```

### 自定义组件

```jsx
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <View style={{ alignItems: 'center', marginTop: 50 }}>
        {/* Greeting组件中将name作为一个属性来定制 */}
        <Text>Hello {this.props.name}!</Text>
      </View>
    );
  }
}

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{ alignItems: 'center' }}>
        <Greeting name='Rexxar' />
        <Greeting name='Jaina' />
        <Greeting name='Valeera' />
      </View>
    );
  }
}
```

## RN 中的 State

一般来说，你需要在 class 中声明一个`state`对象，然后在需要修改时调用`setState`方法。

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个`prop`。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到`state`中。

```jsx
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Blink extends Component {
  // 声明state对象
  state = { isShowingText: true };

  componentDidMount() {
    // 每1000毫秒对showText状态做一次取反操作
    setInterval(() => {
      this.setState({
        isShowingText: !this.state.isShowingText
      });
    }, 1000);
  }

  render() {
    // 根据当前showText的值决定是否显示text内容
    if (!this.state.isShowingText) {
      return null;
    }

    return <Text>{this.props.text}</Text>;
  }
}

export default class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
        <Blink text='Yes blinking is so great' />
        <Blink text='Why did they ever take this out of HTML' />
        <Blink text='Look at me look at me look at me' />
      </View>
    );
  }
}
```

实际开发中，==我们一般不会在定时器函数（setInterval、setTimeout 等）中来操作 state==。典型的场景是在接收到服务器返回的新数据，或者在用户输入数据之后。你也可以使用一些“状态容器”比如[Redux](http://redux.js.org/index.html)来统一管理数据流。

- 一切界面变化都是`状态state变化`
- `state`的修改必须通过`setState()`方法
  - this.state.likes = 100; // 这样的`直接赋值修改无效！`
  - setState 是一个 merge 合并操作，只修改指定属性，不影响其他属性
  - setState 是`异步`操作，修改`不会马上生效`

## 处理文本输入

[`TextInput`](https://reactnative.cn/docs/textinput#content)是一个允许用户输入文本的基础组件。

它有一个名为`onChangeText`的属性，此属性接受一个函数，而此函数会在文本变化时被调用。

另外还有一个名为`onSubmitEditing`的属性，会在文本被提交后（用户按下软键盘上的提交键）调用。

```jsx
/** 处理文本输入
 	每输入一个单词就得到一块 🍕 */
function PizzaTranslator() {
  const [text, setText] = useState('');
  return (
    <View style={{ padding: 10 }}>
      <TextInput style={{ height: 40, backgroundColor: '#ccc' }} placeholder='Type here to translate!' onChangeText={text => setText(text)} defaultValue={text} />
      <Text style={{ padding: 10, fontSize: 42 }}>
        {text
          .split(' ')
          .map(word => word && '🍕')
          .join(' ')}
      </Text>
    </View>
  );
}
```

<img src="/image-20200327083821128.png" alt="image-20200327083821128" style="zoom:50%;" />

## 处理触摸事件

### onPress 事件

```jsx
<Button
  onPress={() => {
    Alert.alert('你点击了按钮！');
  }}
  title='点我！'
/>
```

<img src="/image-20200327084507790.png" alt="image-20200327084507790" style="zoom:50%;" />

```jsx
class App extends Component {
  _onPressButton() {
    Alert.alert('You tapped the button!');
  }
  render() {
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center'
      },
      buttonContainer: {
        margin: 20
      },
      alternativeLayoutButtonContainer: {
        margin: 20,
        flexDirection: 'row',
        justifyContent: 'space-between'
      }
    });
    return (
      <View style={styles.container}>
        <View style={styles.buttonContainer}>
          <Button onPress={this._onPressButton} title='Press Me' />
        </View>
        <View style={styles.buttonContainer}>
          <Button onPress={this._onPressButton} title='Press Me' color='#841584' />
        </View>
        <View style={styles.alternativeLayoutButtonContainer}>
          <Button onPress={this._onPressButton} title='This looks great!' />
          <Button onPress={this._onPressButton} title='OK!' color='#841584' />
        </View>
      </View>
    );
  }
}
```

<img src="/image-20200327085101363.png" alt="image-20200327085101363" style="zoom:50%;" />

### Touchable 系列组件

- 一般来说，你可以使用[**TouchableHighlight**](https://reactnative.cn/docs/touchablehighlight)来制作按钮或者链接。注意此组件的背景会在用户手指按下时变暗。
- 在 Android 上还可以使用[**TouchableNativeFeedback**](https://reactnative.cn/docs/touchablenativefeedback)，它会在用户手指按下时形成类似墨水涟漪的视觉效果。
- [**TouchableOpacity**](https://reactnative.cn/docs/touchableopacity)会在用户手指按下时降低按钮的透明度，而不会改变背景的颜色。
- 如果你想在处理点击事件的同时不显示任何视觉反馈，则需要使用[**TouchableWithoutFeedback**](https://reactnative.cn/docs/touchablewithoutfeedback)。

* 检测用户是否进行了长按，组件中使用`onLongPress`属性来实现。

```jsx
import React, { Component } from 'react';
import { Alert, Platform, StyleSheet, Text, TouchableHighlight, TouchableOpacity, TouchableNativeFeedback, TouchableWithoutFeedback, View } from 'react-native';

class App extends Component {
  _onPressButton() {
    Alert.alert('You tapped the button!');
  }

  _onLongPressButton() {
    Alert.alert('You long-pressed the button!');
  }
  render() {
    const styles = StyleSheet.create({
      container: {
        paddingTop: 60,
        alignItems: 'center'
      },
      button: {
        marginBottom: 30,
        width: 260,
        alignItems: 'center',
        backgroundColor: '#2196F3'
      },
      buttonText: {
        textAlign: 'center',
        padding: 20,
        fontSize: 16,
        color: '#fff'
      }
    });
    return (
      <View style={styles.container}>
        {/* 组件的背景会在用户手指按下时变暗。 */}
        <TouchableHighlight onPress={this._onPressButton} underlayColor='white'>
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableHighlight</Text>
          </View>
        </TouchableHighlight>
        {/* 用户手指按下时降低按钮的透明度，而不会改变背景的颜色。 */}
        <TouchableOpacity onPress={this._onPressButton}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableOpacity</Text>
          </View>
        </TouchableOpacity>
        {/* Android 用户手指按下时形成类似墨水涟漪的视觉效果。 */}
        <TouchableNativeFeedback onPress={this._onPressButton} background={Platform.OS === 'android' ? TouchableNativeFeedback.SelectableBackground() : ''}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableNativeFeedback</Text>
          </View>
        </TouchableNativeFeedback>
        {/* 不显示任何视觉反馈 */}
        <TouchableWithoutFeedback onPress={this._onPressButton}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableWithoutFeedback</Text>
          </View>
        </TouchableWithoutFeedback>
        {/* 绑定单击 和 长按事件 */}
        <TouchableHighlight onPress={this._onPressButton} onLongPress={this._onLongPressButton} underlayColor='white'>
          <View style={styles.button}>
            <Text style={styles.buttonText}>Touchable with Long Press</Text>
          </View>
        </TouchableHighlight>
      </View>
    );
  }
}
```

## 滚动视图

[`ScrollView`](https://reactnative.cn/docs/scrollview)是一个通用的可滚动的容器，你可以在其中放入多个组件和视图，而且这些组件并不需要是同类型的。ScrollView 不仅可以垂直滚动，还能水平滚动（通过`horizontal`属性来设置）。

下面的示例代码创建了一个垂直滚动的`ScrollView`，其中还混杂了图片和文字组件。

<img src="/ScrollView.gif" alt="ScrollView" style="zoom:50%;" />

```jsx
class App extends Component {
  render() {
    return (
      <ScrollView style={{ backgroundColor: '#df02d0' }}>
        <Text style={{ fontSize: 48 }}>Scroll me plz</Text>
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Text style={{ fontSize: 48 }}>If you like</Text>
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Text style={{ fontSize: 48 }}>Scrolling down</Text>
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Text style={{ fontSize: 48 }}>What's the best</Text>
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Text style={{ fontSize: 48 }}>Framework around?</Text>
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Image source={require('./1.png')} style={{ width: 100, height: 100, borderColor: 'gray', borderWidth: 1 }} />
        <Text style={{ fontSize: 80 }}>React Native</Text>
      </ScrollView>
    );
  }
}
```

`ScrollView`适合用来显示数量不多的滚动元素。放置在`ScrollView`中的所有组件都会被渲染，哪怕有些组件因为内容太长被挤出了屏幕外。

## 长列表

`FlatList`组件用于显示一个==垂直的滚动列表==，其中的元素之间结构近似而仅数据不同。

`FlatList`更适于==长列表数据，且元素个数可以增删。==和[`ScrollView`](https://reactnative.cn/docs/using-a-scrollview)不同的是，`FlatList`并==不立即渲染所有元素，而是优先渲染屏幕上可见的元素。==

`FlatList`组件必须的两个属性是`data`和`renderItem`。`data`是列表的数据源，而`renderItem`则从数据源中逐个解析数据，然后返回一个设定好格式的组件来渲染。

<img src="/FlatList.gif" alt="FlatList" style="zoom:50%;" />

```jsx
class App extends Component {
  render() {
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        paddingTop: 22
      },
      item: {
        padding: 10,
        fontSize: 18,
        height: 44,
        borderBottomColor: '#f0f',
        borderBottomWidth: 1
      }
    });
    const arr = [{ key: 'Devin' }, { key: 'Julie' }, { key: 'Dan' }, { key: 'Dominic' }, { key: 'Jackson' }, { key: 'James' }, { key: 'Joel' }, { key: 'John' }, { key: 'Jillian' }, { key: 'Jimmy' }, { key: 'Dan' }, { key: 'Dominic' }, { key: 'Jackson' }, { key: 'James' }, { key: 'Joel' }, { key: 'John' }, { key: 'Jillian' }, { key: 'Jimmy' }];
    return (
      <View style={styles.container}>
        <FlatList data={arr} renderItem={({ item }) => <Text style={styles.item}>{item.key}</Text>} />
      </View>
    );
  }
}
```

如果要渲染的是一组需要分组的数据，也许还带有分组标签的，那么`SectionList`将是个不错的选择

<img src="/SectionList.gif" alt="SectionList" style="zoom:50%;" />

```jsx
class App extends Component {
  render() {
    const arr = [
      { title: 'A', data: ['Aevin', 'Aan', 'Aominic'] },
      { title: 'B', data: ['Bevin', 'Ban', 'Bominic'] },
      { title: 'D', data: ['Devin', 'Dan', 'Dominic'] },
      { title: 'J', data: ['Jackson', 'James', 'Jillian', 'Jimmy', 'Joel', 'John', 'Julie'] }
    ];
    return (
      <View style={styles.container}>
        <SectionList sections={arr} renderItem={({ item }) => <Text style={styles.item}>{item}</Text>} renderSectionHeader={({ section }) => <Text style={styles.sectionHeader}>{section.title}</Text>} keyExtractor={(item, index) => index} />
      </View>
    );
  }
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: 22
  },
  sectionHeader: {
    paddingTop: 2,
    paddingLeft: 10,
    paddingRight: 10,
    paddingBottom: 2,
    fontSize: 14,
    fontWeight: 'bold',
    backgroundColor: 'rgba(247,247,247,1.0)',
    borderTopWidth: 1,
    borderTopColor: '#db5420'
  },
  item: {
    padding: 10,
    fontSize: 18,
    height: 44
  }
});
```

## 网络（进行中）

### 使用 Fetch

#### 发起请求

```js
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue'
  })
});
```

#### 处理服务器的响应数据

Fetch 方法会返回一个[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)，这种模式可以简化异步风格的代码

```js
function getMoviesFromApiAsync() {
  return fetch('https://facebook.github.io/react-native/movies.json')
    .then(response => response.json())
    .then(responseJson => {
      return responseJson.movies;
    })
    .catch(error => {
      console.error(error);
    });
}

// async/await 语法：
// 注意这个方法前面有async关键字
async function getMoviesFromApi() {
  try {
    // 注意这里的await语句，其所在的函数必须有async关键字声明
    let response = await fetch('https://facebook.github.io/react-native/movies.json');
    let responseJson = await response.json();
    return responseJson.movies;
  } catch (error) {
    console.error(error);
  }
}
```

测试(catch 住`fetch`可能抛出的异常)

```jsx
class App extends Component {
  constructor(props) {
    super(props);
    this.state = { isLoading: true };
  }
  componentDidMount() {
    //  *****************
    return fetch('https://facebook.github.io/react-native/movies.json')
      .then(response => response.json())
      .then(responseJson => {
        this.setState(
          {
            isLoading: false,
            dataSource: responseJson.movies
          },
          function () {}
        );
      })
      .catch(error => {
        console.error(error);
      });
  }
  render() {
    if (this.state.isLoading) {
      return (
        <View style={{ flex: 1, padding: 20 }}>
          <ActivityIndicator />
        </View>
      );
    }
    return (
      <View style={{ flex: 1, paddingTop: 20 }}>
        <FlatList
          data={this.state.dataSource}
          renderItem={({ item }) => (
            <Text>
              {item.title}, {item.releaseYear}
            </Text>
          )}
          keyExtractor={(item, index) => item.id}
        />
      </View>
    );
  }
}
```

> 从 Android9 开始，也会默认阻止 http 请求，请参考[相关配置](https://blog.csdn.net/qq_40347548/article/details/86766932)

> 默认情况下，iOS 会阻止所有 http 的请求，以督促开发者使用 https。如果你仍然需要使用 http 协议，那么首先需要添加一个 App Transport Security 的例外，详细可参考[这篇帖子](https://segmentfault.com/a/1190000002933776)。

### 使用其他的网络库

React Native 中已经内置了[XMLHttpRequest API](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)(也就是俗称的 ajax)。一些基于 XMLHttpRequest 封装的第三方库也可以使用，例如[frisbee](https://github.com/niftylettuce/frisbee)或是[axios](https://github.com/mzabriskie/axios)等。但注意不能使用 jQuery。

> 在应用中你可以访问任何网站，没有[跨域](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)的限制。

## 电影列表

> 在一个`FlatList`中展示出电影列表。

### 准备工作

使用`cli`工具创建一个工程：

```bash
react-native init SampleAppMovies
```

这条命令除了会下载`js`文件，还会下载在`SampleAppMovies/iOS/SampleAppMovies.xcodeproj`和`SampleAppMovies/android/app`下分别创建一个新的 XCode 工程(iOS)和一个 gradle 工程(Android)。

### 开发

`Android`版本：先连接设备或启动模拟器，然后在`SampleAppMovies`目录下运行`react-native run-android`，就会构建工程。

#### 电影列表

<img src="/movielist.gif" alt="movielist" style="zoom:50%;" />

```js
import React, { useState, Component } from 'react';
import { View, Text, Button, Alert, Platform, Image, ScrollView, TextInput, FlatList, SectionList, ActivityIndicator, StyleSheet, TouchableHighlight, TouchableOpacity, TouchableNativeFeedback, TouchableWithoutFeedback } from 'react-native';

// 原 url
const REQUEST_URL = 'https://raw.githubusercontent.com/facebook/react-native/0.51-stable/docs/MoviesExample.json';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      data: [],
      loaded: false
    };
    this.fetchData = this.fetchData.bind(this);
  }
  // 请求数据
  fetchData() {
    let data = require('./test/moivelist.json');
    // 通过定时器 模拟ajax
    setTimeout(() => {
      this.setState({
        data: data.movies,
        loaded: true
      });
    }, 3000);
    // 真正请求的ajax
    // fetch(REQUEST_URL)
    //   .then((response) => response.json())
    //   .then((responseData) => {
    //     // 注意，这里使用了this关键字，为了保证this在调用时仍然指向当前组件，我们需要对其进行“绑定”操作
    //     this.setState({
    //       movies: responseData.movies,
    //     });
    //   });
  }
  // 在生命周期中调用接口
  componentDidMount() {
    this.fetchData();
  }
  render() {
    if (!this.state.loaded) {
      return this.renderLoadingView();
    }
    return <FlatList data={this.state.data} renderItem={this.renderMovie} style={styles.list} keyExtractor={item => item.id} />;
  }
  // 加载动画
  renderLoadingView() {
    return (
      <View style={styles.container}>
        <Text>正在加载电影数据……</Text>
      </View>
    );
  }
  // 渲染列表
  renderMovie({ item }) {
    // { item }是一种“解构”写法，请阅读ES2015语法的相关文档
    // item也是FlatList中固定的参数名，请阅读FlatList的相关文档
    return (
      <View style={styles.container}>
        <Image source={{ uri: item.posters.thumbnail }} style={styles.thumbnail} />
        <View style={styles.rightContainer}>
          <Text style={styles.title}>{item.title}</Text>
          <Text style={styles.year}>{item.year}</Text>
        </View>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  list: {
    paddingTop: 20,
    backgroundColor: '#F5FCFF'
  },
  container: {
    flex: 1,
    flexDirection: 'row',
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF'
  },
  rightContainer: {
    flex: 1
  },
  title: {
    fontSize: 20,
    marginBottom: 8,
    textAlign: 'center'
  },
  year: {
    textAlign: 'center'
  },
  thumbnail: {
    width: 53,
    height: 81
  }
});

export default App;
```

# 设计

## RN 中的样式

所有的核心组件都接受名为`style`的属性。这些样式名基本上是遵循了 web 上的 CSS 的命名，只是按照 JS 的语法要求使用了驼峰命名法，例如将`background-color`改为`backgroundColor`。

- `style`属性可以是一个普通的 JavaScript 对象

- 使用`StyleSheet.create`来集中定义组件的样式。

```jsx
import React, { Component } from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default class LotsOfStyles extends Component {
  render() {
    return (
      <View>
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigBlue}>just bigBlue</Text>
        <Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>
        <Text style={[styles.red, styles.bigBlue]}>red, then bigBlue</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  bigBlue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30
  },
  red: {
    color: 'red'
  }
});
```

## 高度与宽度

组件的高度和宽度决定了其在屏幕上显示的尺寸。

### 指定宽高

样式中指定固定的`width`和`height`。React Native 中的尺寸都是无单位的，表示的是与设备像素密度无关的逻辑像素点。

```jsx
import React, { Component } from 'react';
import { View } from 'react-native';

export default class FixedDimensionsBasics extends Component {
  render() {
    return (
      <View>
        <View style={{ width: 50, height: 50, backgroundColor: 'powderblue' }} />
        <View style={{ width: 100, height: 100, backgroundColor: 'skyblue' }} />
        <View style={{ width: 150, height: 150, backgroundColor: 'steelblue' }} />
      </View>
    );
  }
}
```

这样给组件设置尺寸也是一种常见的模式，比如要求在不同尺寸的屏幕上都显示成一样的大小。

<img src="/image-20200326151335084.png" alt="image-20200326151335084" style="zoom:50%;" />

**注意：此时没法适配多种屏幕大小**

### 弹性（Flex）宽高

在组件样式中使用`flex`可以使其在可利用的空间中动态地扩张或收缩。一般而言我们会使用`flex:1`来指定某个组件扩张以撑满所有剩余的空间。如果有多个并列的子组件使用了`flex:1`，则这些子组件会平分父容器中剩余的空间。如果这些并列的子组件的`flex`值不一样，则谁的值更大，谁占据剩余空间的比例就更大（即占据剩余空间的比等于并列组件间`flex`值的比）。

```jsx
const App = () => {
  return (
    <View style={styles.father}>
      <View style={styles.son1} />
      <View style={styles.son2} />
      <View style={styles.son3} />
    </View>
  );
};
// 去掉父View中的`flex: 1`。  ----- View不再具有尺寸，因此子组件也无法再撑开。
// 然后再用`height: 300`来代替父View的`flex: 1` ---- 父View的高度变成300
const styles = StyleSheet.create({
  father: {
    flex: 1
    // flexDirection: "row", // 改变子元素的伸缩方向
  },
  son1: {
    flex: 1,
    backgroundColor: 'powderblue'
  },
  son2: {
    flex: 2,
    backgroundColor: 'skyblue'
  },
  son3: {
    flex: 3,
    backgroundColor: 'steelblue'
  }
});
```

<img src="/image-20200326151248022.png" alt="image-20200326151248022" style="zoom:50%;" />

## 使用 Flexbox 布局

Flexbox 可以在不同屏幕尺寸上提供一致的布局结构。

使用`flexDirection`、`alignItems`和 `justifyContent`三个样式属性就已经能满足大多数布局需求。

> React Native 中的 Flexbox 的工作原理和 web 上的 CSS 基本一致，当然也存在少许差异。首先是默认值不同：`flexDirection`的默认值是`column`而不是`row`，而`flex`也只能指定一个数字值。

### Flex

如上

### Flex Direction

`flexDirection`可以决定布局的**主轴**。**水平轴(`row`)**方向，竖直轴(`column`)**方向。默认值是**竖直轴(`column`)方向。

```jsx
class App extends Component {
  render() {
    return (
      // 尝试把`flexDirection`改为`column`看看
      <View style={{ flex: 1, flexDirection: 'row' }}>
        <View style={{ width: 50, height: 50, backgroundColor: 'powderblue' }} />
        <View style={{ width: 50, height: 50, backgroundColor: 'skyblue' }} />
        <View style={{ width: 50, height: 50, backgroundColor: 'steelblue' }} />
      </View>
    );
  }
}
```

<img src="/image-20200326154915534.png" alt="image-20200326154915534" style="zoom:50%;" />

### 布局方向

**LTR：文本和子项，从左到右排列**

布局方向指定层次结构中子级和文本的布局方向。 布局方向也会影响边的起点和终点。

默认情况下，React Native 使用 LTR 布局方向进行布局。 在此模式下，开始是指左，结束是指右。

LTR（默认值）文本和子项，从左到右排列。 元素的开始处应用的边距和填充在左侧。

RTL 文本和子级，从右到左排列。 应用于元素开始的边距和填充在右侧。

### Justify Content

```jsx
class App extends Component {
  render() {
    return (
      // 尝试把`justifyContent`改为`center`看看
      // 尝试把`flexDirection`改为`row`看看
      <View
        style={{
          flex: 1,
          flexDirection: 'column', // 默认纵向布局
          justifyContent: 'space-between'
        }}
      >
        <View style={{ width: 100, height: 100, backgroundColor: 'powderblue' }} />
        <View style={{ width: 100, height: 100, backgroundColor: 'skyblue' }} />
        <View style={{ width: 100, height: 100, backgroundColor: 'steelblue' }} />
      </View>
    );
  }
}
```

<img src="/image-20200326155234896.png" alt="image-20200326155234896" style="zoom:50%;" />

### Align Items

```jsx
class App extends Component {
  render() {
    return (
      // 尝试把`alignItems`改为`flex-start`看看
      // 尝试把`justifyContent`改为`flex-end`看看
      // 尝试把`flexDirection`改为`row`看看
      <View
        style={{
          flex: 1,
          flexDirection: 'column',
          justifyContent: 'center',
          alignItems: 'stretch'
        }}
      >
        <View style={{ width: 50, height: 50, backgroundColor: 'powderblue' }} />
        <View style={{ height: 50, backgroundColor: 'skyblue' }} />
        <View style={{ height: 100, backgroundColor: 'steelblue' }} />
      </View>
    );
  }
}
```

<img src="/image-20200326155524158.png" alt="image-20200326155524158" style="zoom:50%;" />

### Align Self

alignSelf 具有与 alignItems 相同的选项和效果，但是不影响容器中的子元素，您可以将此属性应用于单个子元素，以更改其在其父元素中的对齐方式。alignSelf 用 alignItems 覆盖父类设置的任何选项。

```jsx
class App extends Component {
  render() {
    return (
      <View
        style={{
          flex: 1,
          flexDirection: 'column',
          justifyContent: 'center',
          alignItems: 'stretch'
        }}
      >
        <View style={{ height: 100, backgroundColor: 'powderblue' }}>
          <Text style={{ alignSelf: 'flex-end', weight: 50, height: 50, backgroundColor: 'red' }}>子内容</Text>
        </View>
        <View style={{ height: 50, backgroundColor: 'skyblue' }} />
        <View style={{ height: 100, backgroundColor: 'steelblue' }} />
      </View>
    );
  }
}
```

**alignSelf: `flex-start|flex-end|center|stretch`**

<img src="/image-20200326160914205.png" alt="image-20200326160914205" style="zoom:50%;" />

### Align Content

alignContent 定义沿横轴的线分布。 仅当使用 flexWrap 将项目包装到多行时，此功能才有效。

- `flex-start`（默认值）将换行对齐到容器横轴的起点。
- `flex-end`将换行对齐到容器横轴的末端。
- `stretch` 包装线以匹配容器横轴的高度。
- `center`将包装线对齐到容器横轴的中心。
- `space-between`在容器的主轴上均匀间隔的线，在线之间分配剩余空间。
- `space-around`在容器的主轴上均匀间隔地包裹线，在线周围分配剩余空间。 与使用空格之间的空间相比，周围的空间将导致空间分配到第一行的开头和最后一行的末尾。
- `space-evenly` 沿主轴均匀地分布在对齐容器内。 每对相邻项之间的间距，主开始边缘和第一个项目以及主结束边缘和最后一个项目都完全相同。

<img src="/image-20200331154557560.png" alt="image-20200331154557560" style="zoom:67%;" />

### Flex Wrap

flexWrap 属性在容器上设置，并控制当子原生沿主轴溢出容器大小时发生的情况。 默认情况下，子级被强制为一行（可以收缩元素）。 如果允许包装，则根据需要将项目沿主轴包装成多行。

换行时，可以使用 alignContent 指定行在容器中的放置方式。

<img src="/image-20200331154722643.png" alt="image-20200331154722643" style="zoom:50%;" />

### Flex Basis, Grow, and Shrink

- flexGrow 描述如何沿主轴在容器的子级之间分配容器中的任何空间。 布置好其子项后，容器将根据其子项指定的伸缩增长值分配所有剩余空间。

  - flexGrow 接受任何大于等于 0 的浮点值，其中 0 为默认值。 容器将在孩子之间分配剩余空间，该剩余空间将根据孩子的弹性增长值来加权。

- flexShrink 描述了在子项的总大小溢出主轴上容器大小的情况下如何沿主轴收缩子项。 Flex 收缩与 Flex 增长非常相似，如果任何溢出大小都被视为负剩余空间，则可以用相同的方式来考虑。 通过允许孩子根据需要生长和收缩，这两个属性也可以很好地协同工作。

  - Flex 收缩接受任何大于等于 0 的浮点值，默认值为 1。 容器将根据其子项的 flex 收缩值对子项进行收缩。

- flexBasis 是一种与轴无关的方式，用于沿主轴提供项目的默认大小。 如果子项的父级是具有 flexDirection：row 的容器，则设置子项的 flex 基础类似于设置子项的宽度；如果子项的父级是具有 flexDirection：列的容器，则设置子项的高度。 项目的伸缩基础是该项目的默认大小，即执行任何伸缩增长和伸缩收缩计算之前的项目大小。

### 宽度和高度

Yoga 中的 width 属性指定元素内容区域的宽度。 同样，height 属性指定元素内容区域的高度。

宽度和高度都可以采用以下值：

- auto 是默认值，React Native 根据元素的内容（无论是其他子元素，文本还是图像）计算元素的宽度/高度。

- pixel 定义宽度/高度，以绝对像素为单位。 根据组件上设置的其他样式，这可能是节点的最终尺寸，也可能不是。

- percent 定义宽度或高度，以其父级宽度或高度的百分比表示。

### 绝对和相对布局

元素的位置类型定义了元素在其父元素中的位置。

相对（默认值）默认情况下，元素是相对放置的。 这意味着元素将根据布局的正常流程进行定位，然后根据上，右，下和左的值相对于该位置偏移。 偏移量不会影响任何同级元素或父元素的位置。

绝对当绝对定位时，元素不参与正常的布局流程。 相反，它的布局与兄弟姐妹无关。 根据顶部，右侧，底部和左侧的值确定位置。

<img src="/image-20200331155044160.png" alt="image-20200331155044160" style="zoom:50%;" />
