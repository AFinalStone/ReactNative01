## React Native学习<三> ReactNative入门基础

### <一>编写Hello World

React Native看起来很像React，只不过其基础组件是原生组件而非web组件。要理解React Native应用的基本结构，首先需要了解一些基本的React的概念，比如JSX语法、组件、`state`状态以及`props`属性。如果你已经了解了React，那么还需要掌握一些React Native特有的知识，比如原生组件的使用。这篇教程可以供任何基础的读者学习，不管你是否有React方面的经验。

让我们开始吧！

#### Hello World

根据历史悠久的“传统”，我们也来写一个“Hello World”：

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, Text } from 'react-native';

class HelloWorldApp extends Component {
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}

// 注意，这里用引号括起来的'HelloWorldApp'必须和你init创建的项目名一致
AppRegistry.registerComponent('HelloWorld_ReactNative', () => HelloWorldApp);
```

你可以新建一个项目，然后用上面的代码覆盖你的`index.ios.js`或是`index.android.js` 文件，然后运行看看。

#### 那这段代码是什么意思呢？

初看这段代码，可能觉得并不像JavaScript——没错，这是“未来”的JavaScript.

首先你需要了解ES2015 （也叫作ES6）——这是一套对JavaScript的语法改进的官方标准。但是这套标准目前还没有在所有的浏览器上完整实现，所以目前而言web开发中还很少使用。React Native内置了对ES2015标准的支持，你可以放心使用而无需担心兼容性问题。上面的示例代码中的`import`、`from`、`class`、`extends`、以及`() =>`箭头函数等新语法都是ES2015中的特性。如果你不熟悉ES2015的话，可以看看[阮一峰老师的书](http://es6.ruanyifeng.com/)，还有论坛的这篇[总结](http://bbs.reactnative.cn/topic/15)。

示例中的这一行`<Text>Hello world!</Text>`恐怕很多人看起来也觉得陌生。这叫做JSX——是一种在JavaScript中嵌入XML结构的语法。很多传统的应用框架会设计自有的模板语法，让你在结构标记中嵌入代码。React反其道而行之，设计的JSX语法却是让你在代码中嵌入结构标记。初看起来，这种写法很像web上的HTML，只不过使用的并不是web上常见的标签如`<div>`或是`<span>`等，这里我们使用的是React Native的组件。上面的示例代码中，使用的是内置的`<Text>`组件，它专门用来显示文本。

#### 组件与AppRegistry

上面的代码定义了一个名为`HelloWorldApp`的新的`组件（Component）`，并且使用了名为`AppRegistry`的内置模块进行了“注册”操作。你在编写React Native应用时，肯定会写出很多新的组件。而一个App的最终界面，其实也就是各式各样的组件的组合。组件本身结构可以非常简单——唯一必须的就是在`render`方法中返回一些用于渲染结构的JSX语句。

`AppRegistry`模块则是用来告知React Native哪一个组件被注册为整个应用的根容器。你无需在此深究，因为一般在整个应用里`AppRegistry.registerComponent`这个方法只会调用一次。上面的代码里已经包含了具体的用法，你只需整个复制到`index.ios.js`或是`index.android.js`文件中即可运行。

#### 这个示例弱爆了！

……是的。如果想做些更有意思的东西，请[接着学习Props属性](props.html)。或者可以看看一个[稍微复杂些的“电影列表”例子](sample-application-movies.html)。

### <二>Props（属性）

大多数组件在创建时就可以使用各种参数来进行定制。用于定制的这些参数就称为`props`（属性）。

以常见的基础组件`Image`为例，在创建一个图片时，可以传入一个名为`source`的prop来指定要显示的图片的地址，以及使用名为`style`的prop来控制其尺寸。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, Image } from 'react-native';

class Bananas extends Component {
  render() {
    let pic = {
      uri: 'http://pic.duowan.com/xunxian/1102/161618998125/161620264846.jpg'
    };
    return (
      <Image source={pic} style={{width: 193, height: 110}} />
    );
  }
}

AppRegistry.registerComponent('HelloWorld_ReactNative', () => Bananas);
```

译注：在iOS上使用http链接的图片地址可能不会显示，参见[这篇说明修改](https://segmentfault.com/a/1190000002933776)。

请注意`{pic}`外围有一层括号，我们需要用括号来把`pic`这个变量嵌入到JSX语句中。括号的意思是括号内部为一个js变量或表达式，需要执行后取值。因此我们可以把任意合法的JavaScript表达式通过括号嵌入到JSX语句中。

自定义的组件也可以使用`props`。通过在不同的场景使用不同的属性定制，可以尽量提高自定义组件的复用范畴。只需在`render`函数中引用`this.props`，然后按需处理即可。下面是一个例子：

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <Text>Hello {this.props.name}!</Text>
    );
  }
}

class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Greeting name='Rexxar' />
        <Greeting name='Jaina' />
        <Greeting name='Valeera' />
      </View>
    );
  }
}

AppRegistry.registerComponent('HelloWorld_ReactNative', () => LotsOfGreetings);
```

我们在`Greeting`组件中将`name`作为一个属性来定制，这样可以复用这一组件来制作各种不同的“问候语”。上面的例子把`Greeting`组件写在JSX语句中，用法和内置组件并无二致——这正是React体系的魅力所在——如果你想搭建一套自己的基础UI框架，那就放手做吧！

上面的例子出现了一样新的名为[`View`](view.html)的组件。[`View`](view.html) 常用作其他组件的容器，来帮助控制布局和样式。

仅仅使用`props`和基础的[`Text`](text.html)、[`Image`](image.html)以及[`View`](view.html)组件，你就已经足以编写各式各样的UI组件了。要学习如何动态修改你的界面，那就需要进一步[学习State（状态）的概念](state.html)。

### <三>State（状态）

我们使用两种数据来控制一个组件：`props`和`state`。`props`是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变。 对于需要改变的数据，我们需要使用`state`。

一般来说，你需要在constructor中初始化`state`（译注：这是ES6的写法，早期的很多ES5的例子使用的是getInitialState方法来初始化state，这一做法会逐渐被淘汰），然后在需要修改时调用`setState`方法。

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个`prop`。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到`state`中。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Blink extends Component {
  constructor(props) {
    super(props);
    this.state = { showText: true };

    // 每1000毫秒对showText状态做一次取反操作
    setInterval(() => {
      this.setState(previousState => {
        return { showText: !previousState.showText };
      });
    }, 1000);
  }

  render() {
    // 根据当前showText的值决定是否显示text内容
    let display = this.state.showText ? this.props.text : ' ';
    return (
      <Text>{display}</Text>
    );
  }
}

class BlinkApp extends Component {
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

AppRegistry.registerComponent('HelloWorld_ReactNative', () => BlinkApp);
```

实际开发中，我们一般不会在定时器函数（setInterval、setTimeout等）中来操作state。典型的场景是在接收到服务器返回的新数据，或者在用户输入数据之后。你也可以使用一些“状态容器”比如[Redux](http://redux.js.org/index.html)来统一管理数据流（译注：但我们不建议新手过早去学习redux）。

State的工作原理和React.js完全一致，所以对于处理state的一些更深入的细节，你可以参阅[React.Component API](https://facebook.github.io/react/docs/component-api.html)。

看到这里，你可能觉得我们的例子总是千篇一律的黑色文本，太特么无聊了。那么我们一起来[学习一下样式](style.html)吧。

### <四> 樣式

在React Native中，你并不需要学习什么特殊的语法来定义样式。我们仍然是使用JavaScript来写样式。所有的核心组件都接受名为`style`的属性。这些样式名基本上是遵循了web上的CSS的命名，只是按照JS的语法要求使用了驼峰命名法，例如将`background-color`改为`backgroundColor`。

`style`属性可以是一个普通的JavaScript对象。这是最简单的用法，因而在示例代码中很常见。你还可以传入一个数组——在数组中位置居后的样式对象比居前的优先级更高，这样你可以间接实现样式的继承。

实际开发中组件的样式会越来越复杂，我们建议使用`StyleSheet.create`来集中定义组件的样式。比如像下面这样：

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, StyleSheet, Text, View } from 'react-native';

class LotsOfStyles extends Component {
  render() {
    return (
      <View>
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigblue}>just bigblue</Text>
        <Text style={[styles.bigblue, styles.red]}>bigblue, then red</Text>
        <Text style={[styles.red, styles.bigblue]}>red, then bigblue</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  bigblue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});

AppRegistry.registerComponent('HelloWorld_ReactNative', () => LotsOfStyles);
```

常见的做法是按顺序声明和使用`style`属性，以借鉴CSS中的“层叠”做法（即后声明的属性会覆盖先声明的同名属性）。

文本的样式定义请参阅[Text组件的文档](text.html)。

现在你已经了解如何调整文本样式了，下面我们要学习的是[如何控制组件的尺寸](height-and-width.html)。

### <五> 高度和宽度

组件的高度和宽度决定了其在屏幕上显示的尺寸。

#### 指定宽高

最简单的给组件设定尺寸的方式就是在样式中指定固定的`width`和`height`。React Native中的尺寸都是无单位的，表示的是与设备像素密度无关的逻辑像素点。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class FixedDimensionsBasics extends Component {
 render() {
   return (
     <View>
       <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
       <View style={{width: 100, height: 100, backgroundColor: 'skyblue'}} />
       <View style={{width: 150, height: 150, backgroundColor: 'steelblue'}} />
     </View>
   );
 }
};
// 注册应用(registerComponent)后才能正确渲染
// 注意：只把应用作为一个整体注册一次，而不是每个组件/模块都注册
AppRegistry.registerComponent('HelloWorld_ReactNative', () => FixedDimensionsBasics);

```

这样给组件设置尺寸也是一种常见的模式，比如要求在不同尺寸的屏幕上都显示成一样的大小。

#### 弹性（Flex）宽高

在组件样式中使用`flex`可以使其在可利用的空间中动态地扩张或收缩。一般而言我们会使用`flex:1`来指定某个组件扩张以撑满所有剩余的空间。如果有多个并列的子组件使用了`flex:1`，则这些子组件会平分父容器中剩余的空间。如果这些并列的子组件的`flex`值不一样，则谁的值更大，谁占据剩余空间的比例就更大（即占据剩余空间的比等于并列组件间`flex`值的比）。

> 组件能够撑满剩余空间的前提是其父容器的尺寸不为零。如果父容器既没有固定的`width`和`height`，也没有设定`flex`，则父容器的尺寸为零。其子组件如果使用了`flex`，也是无法显示的。

```ReactNativeWebPlayer

       import React, { Component } from 'react';
       import { AppRegistry, View } from 'react-native';

       class FlexDimensionsBasics extends Component {
         render() {
           return (
             // 试试去掉父View中的`flex: 1`。
             // 则父View不再具有尺寸，因此子组件也无法再撑开。
             // 然后再用`height: 300`来代替父View的`flex: 1`试试看？
             <View style={{flex: 1}}>
               <View style={{flex: 1, backgroundColor: 'powderblue'}} />
               <View style={{flex: 2, backgroundColor: 'skyblue'}} />
               <View style={{flex: 3, backgroundColor: 'steelblue'}} />
             </View>
           );
         }
       };

       AppRegistry.registerComponent('HelloWorld_ReactNative', () => FlexDimensionsBasics);
```

当你熟练掌握了如何控制组件的尺寸后，下一步可以[学习如何在屏幕上排列组件了](layout-with-flexbox.html)。

### <六> 使用Flexbox布局

我们在React Native中使用flexbox规则来指定某个组件的子元素的布局。Flexbox可以在不同屏幕尺寸上提供一致的布局结构。

一般来说，使用`flexDirection`、`alignItems`和 `justifyContent`三个样式属性就已经能满足大多数布局需求。译注：这里有一份[简易布局图解](http://weibo.com/1712131295/CoRnElNkZ?ref=collection&type=comment)，可以给你一个大概的印象。

> React Native中的Flexbox的工作原理和web上的CSS基本一致，当然也存在少许差异。首先是默认值不同：`flexDirection`的默认值是`column`而不是`row`，而`flex`也只能指定一个数字值。

#### Flex Direction

在组件的`style`中指定`flexDirection`可以决定布局的**主轴**。子元素是应该沿着**水平轴(`row`)**方向排列，还是沿着**竖直轴(`column`)**方向排列呢？默认值是**竖直轴(`column`)**方向。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class FlexDirectionBasics extends Component {
  render() {
    return (
      // 尝试把`flexDirection`改为`column`看看
      <View style={{flex: 1, flexDirection: 'row'}}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('HelloWorld_ReactNative', () => FlexDirectionBasics);
```

#### Justify Content

在组件的style中指定`justifyContent`可以决定其子元素沿着**主轴**的**排列方式**。子元素是应该靠近主轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：`flex-start`、`center`、`flex-end`、`space-around`以及`space-between`。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class JustifyContentBasics extends Component {
  render() {
    return (
      // 尝试把`justifyContent`改为`center`看看
      // 尝试把`flexDirection`改为`row`看看
      <View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'space-between',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('HelloWorld_ReactNative', () => JustifyContentBasics);
```

#### Align Items

在组件的style中指定`alignItems`可以决定其子元素沿着**次轴**（与主轴垂直的轴，比如若主轴方向为`row`，则次轴方向为`column`）的**排列方式**。子元素是应该靠近次轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：`flex-start`、`center`、`flex-end`以及`stretch`。

> 注意：要使`stretch`选项生效的话，子元素在次轴方向上不能有固定的尺寸。以下面的代码为例：只有将子元素样式中的`width: 50`去掉之后，`alignItems: 'stretch'`才能生效。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

class AlignItemsBasics extends Component {
  render() {
    return (
      // 尝试把`alignItems`改为`flex-start`看看
      // 尝试把`justifyContent`改为`flex-end`看看
      // 尝试把`flexDirection`改为`row`看看
      <View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'center',
        alignItems: 'center',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
};

AppRegistry.registerComponent('HelloWorld_ReactNative', () => AlignItemsBasics);
```

#### 深入学习

以上我们已经介绍了一些基础知识，但要运用好布局，我们还需要很多其他的样式。对于布局有影响的完整样式列表记录在[这篇文档中](layout-props.html)。

现在我们已经差不多可以开始真正的开发工作了。哦，忘了还有个常用的知识点：[如何使用TextInput组件来处理用户输入](handling-text-input.html)。

### <七> 处理文本输入

[`TextInput`](textinput.html#content)是一个允许用户输入文本的基础组件。它有一个名为`onChangeText`的属性，此属性接受一个函数，而此函数会在文本变化时被调用。另外还有一个名为`onSubmitEditing`的属性，会在文本被提交后（用户按下软键盘上的提交键）调用。

假如我们要实现当用户输入时，实时将其以单词为单位翻译为另一种文字。我们假设这另一种文字来自某个吃货星球，只有一个单词： 🍕。所以"Hello there Bob"将会被翻译为"🍕🍕🍕"。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, Text, TextInput, View } from 'react-native';

class PizzaTranslator extends Component {
  constructor(props) {
    super(props);
    this.state = {text: ''};
  }

  render() {
    return (
      <View style={{padding: 10}}>
        <TextInput
          style={{height: 40}}
          placeholder="Type here to translate!"
          onChangeText={(text) => this.setState({text})}
        />
        <Text style={{padding: 10, fontSize: 42}}>
          {this.state.text.split(' ').map((word) => word && '🍕').join(' ')}
        </Text>
      </View>
    );
  }
}
// 注册应用(registerComponent)后才能正确渲染
// 注意：只把应用作为一个整体注册一次，而不是每个组件/模块都注册
AppRegistry.registerComponent('HelloWorld_ReactNative', () => PizzaTranslator);
```

在上面的例子里，我们把`text`保存到state中，因为它会随着时间变化。

文本输入方面还有很多其他的东西要处理。比如你可能想要在用户输入的时候进行验证，在[React的表单组件中的受限组件](http://reactjs.cn/react/docs/forms.html)一节中有一些详细的示例（注意react中的onChange对应的是rn中的onChangeText）。此外你还需要看看[TextInput的文档](textinput.html)。

TextInput可能是天然具有“动态状态”的最简单的组件了。下面我们来看看另一类控制布局的组件，先从[ScrollView开始学习](using-a-scrollview.html)。

### <八> 如何使用滚动视图

[`ScrollView`](scrollview.html)是一个通用的可滚动的容器，你可以在其中放入多个组件和视图，而且这些组件并不需要是同类型的。ScrollView不仅可以垂直滚动，还能水平滚动（通过`horizontal`属性来设置）。

下面的示例代码创建了一个垂直滚动的`ScrollView`，其中还混杂了图片和文字组件。

注：下面的这个`./img/favicon.png`并不实际存在，请自己准备图片素材，并改为相对应的正确路径，具体请参考[图片文档](images.html)。

```ReactNativeWebPlayer
import React, { Component } from 'react';
import{ AppRegistry, ScrollView, Image, Text, View } from 'react-native'

class IScrolledDownAndWhatHappenedNextShockedMe extends Component {
  render() {
      return(
        <ScrollView>
          <Text style={{fontSize:96}}>Scroll me plz</Text>
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Text style={{fontSize:96}}>If you like</Text>
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Text style={{fontSize:96}}>Scrolling down</Text>
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Text style={{fontSize:96}}>What's the best</Text>
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Text style={{fontSize:96}}>Framework around?</Text>
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Image source={require('./img/favicon.png')} />
          <Text style={{fontSize:80}}>React Native</Text>
        </ScrollView>
    );
  }
}

// 注册应用(registerComponent)后才能正确渲染
// 注意：只把应用作为一个整体注册一次，而不是每个组件/模块都注册
AppRegistry.registerComponent(
  'HelloWorld_ReactNative',
  () => IScrolledDownAndWhatHappenedNextShockedMe);
```

`ScrollView`适合用来显示数量不多的滚动元素。放置在`ScollView`中的所有组件都会被渲染，哪怕有些组件因为内容太长被挤出了屏幕外。如果你需要显示较长的滚动列表，那么应该使用功能差不多但性能更好的`ListView`组件。下面我们来看看[如何使用ListView](using-a-listview.html)。

### <九> 如何使用长列表

React Native提供了几个适用于展示长列表数据的组件，一般而言我们会选用[FlatList](flatlist.html)或是[SectionList](sectionlist.html)。

`FlatList`组件用于显示一个垂直的滚动列表，其中的元素之间结构近似而仅数据不同。

`FlatList`更适于长列表数据，且元素个数可以增删。和[`ScrollView`](using-a-scrollview.html)不同的是，`FlatList`并不立即渲染所有元素，而是优先渲染屏幕上可见的元素。

`FlatList`组件必须的两个属性是`data`和`renderItem`。`data`是列表的数据源，而`renderItem`则从数据源中逐个解析数据，然后返回一个设定好格式的组件来渲染。

下面的例子创建了一个简单的`FlatList`，并预设了一些模拟数据。首先是初始化`FlatList`所需的`data`，其中的每一项（行）数据之后都在`renderItem`中被渲染成了`Text`组件，最后构成整个`FlatList`。


```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, FlatList, StyleSheet, Text, View } from 'react-native';

export default class FlatListBasics extends Component {
  render() {
    return (
      <View style={styles.container}>
        <FlatList
          data={[
            {key: 'Devin'},
            {key: 'Jackson'},
            {key: 'James'},
            {key: 'Joel'},
            {key: 'John'},
            {key: 'Jillian'},
            {key: 'Jimmy'},
            {key: 'Julie'},
          ]}
          renderItem={({item}) => <Text style={styles.item}>{item.key}</Text>}
        />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
   flex: 1,
   paddingTop: 22
  },
  item: {
    padding: 10,
    fontSize: 18,
    height: 44,
  },
})

// skip this line if using Create React Native App
AppRegistry.registerComponent('HelloWorld_ReactNative', () => FlatListBasics);
```

If you want to render a set of data broken into logical sections, maybe with section headers, then a `SectionList` is the way to go.

```ReactNativeWebPlayer
import React, { Component } from 'react';
import { AppRegistry, SectionList, StyleSheet, Text, View } from 'react-native';

export default class SectionListBasics extends Component {
  render() {
    return (
      <View style={styles.container}>
        <SectionList
          sections={[
            {title: 'D', data: ['Devin']},
            {title: 'J', data: ['Jackson', 'James', 'Jillian', 'Jimmy', 'Joel', 'John', 'Julie']},
          ]}
          renderItem={({item}) => <Text style={styles.item}>{item}</Text>}
          renderSectionHeader={({section}) => <Text style={styles.sectionHeader}>{section.title}</Text>}
        />
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
  },
  item: {
    padding: 10,
    fontSize: 18,
    height: 44,
  },
})

// skip this line if using Create React Native App
AppRegistry.registerComponent('HelloWorld_ReactNative', () => SectionListBasics);
```

列表的一个常用场景就是从服务器端取回列表数据然后显示，要实现这一过程，你可能还需要学习[React Native的网络相关用法](network.html).


### <十> 网络

很多移动应用都需要从远程地址中获取数据或资源。你可能需要给某个REST API发起POST请求以提交用户数据，又或者可能仅仅需要从某个服务器上获取一些静态内容——以下就是你会用到的东西。新手可以对照这个[简短的视频教程](http://v.youku.com/v_show/id_XMTUyNTEwMTA5Ng==.html)加深理解。

## 使用Fetch

React Native提供了和web标准一致的[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)，用于满足开发者访问网络的需求。如果你之前使用过`XMLHttpRequest`(即俗称的ajax)或是其他的网络API，那么Fetch用起来将会相当容易上手。这篇文档只会列出Fetch的基本用法，并不会讲述太多细节，你可以使用你喜欢的搜索引擎去搜索`fetch api`关键字以了解更多信息。

#### 发起网络请求

要从任意地址获取内容的话，只需简单地将网址作为参数传递给fetch方法即可（fetch这个词本身也就是`获取`的意思）：

```js
fetch('https://mywebsite.com/mydata.json')
```

Fetch还有可选的第二个参数，可以用来定制HTTP请求一些参数。你可以指定header参数，或是指定使用POST方法，又或是提交数据等等：

```js
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue',
  })
})
```

译注：如果你的服务器无法识别上面POST的数据格式，那么可以尝试传统的form格式，示例如下：

```js
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
  },
  body: 'key1=value1&key2=value2'
})
```

可以参考[Fetch请求文档](https://developer.mozilla.org/en-US/docs/Web/API/Request)来查看所有可用的参数。

#### 处理服务器的响应数据

上面的例子演示了如何发起请求。很多情况下，你还需要处理服务器回复的数据。

网络请求天然是一种异步操作（译注：同样的还有[asyncstorage](asyncstorage.html)，请不要再问怎样把异步变成同步！无论在语法层面怎么折腾，它们的异步本质是无法变更的。异步的意思是你应该趁这个时间去做点别的事情，比如显示loading，而不是让界面卡住傻等）。Fetch 方法会返回一个[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)，这种模式可以简化异步风格的代码（译注：同样的，如果你不了解promise，建议使用搜索引擎补课）：

  ```js
  getMoviesFromApiAsync() {
    return fetch('https://facebook.github.io/react-native/movies.json')
      .then((response) => response.json())
      .then((responseJson) => {
        return responseJson.movies;
      })
      .catch((error) => {
        console.error(error);
      });
  }
  ```

你也可以在React Native应用中使用ES7标准中的`async`/`await` 语法：

  ```js
  // 注意这个方法前面有async关键字
  async getMoviesFromApi() {
    try {
      // 注意这里的await语句，其所在的函数必须有async关键字声明
      let response = await fetch('https://facebook.github.io/react-native/movies.json');
      let responseJson = await response.json();
      return responseJson.movies;
    } catch(error) {
      console.error(error);
    }
  }
  ```

别忘了catch住`fetch`可能抛出的异常，否则出错时你可能看不到任何提示。

> 默认情况下，iOS会阻止所有非https的请求。如果你请求的接口是http协议，那么首先需要添加一个App Transport Security的例外，详细可参考[这篇帖子](https://segmentfault.com/a/1190000002933776)。


### 使用其他的网络库

React Native中已经内置了[XMLHttpRequest API](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)(也就是俗称的ajax)。一些基于XMLHttpRequest封装的第三方库也可以使用，例如[frisbee](https://github.com/niftylettuce/frisbee)或是[axios](https://github.com/mzabriskie/axios)等。但注意不能使用jQuery，因为jQuery中还使用了很多浏览器中才有而RN中没有的东西（所以也不是所有web中的ajax库都可以直接使用）。

```js
var request = new XMLHttpRequest();
request.onreadystatechange = (e) => {
  if (request.readyState !== 4) {
    return;
  }

  if (request.status === 200) {
    console.log('success', request.responseText);
  } else {
    console.warn('error');
  }
};

request.open('GET', 'https://mywebsite.com/endpoint/');
request.send();
```

> 需要注意的是，安全机制与网页环境有所不同：在应用中你可以访问任何网站，没有[跨域](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)的限制。

## WebSocket支持

React Native还支持[WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)，这种协议可以在单个TCP连接上提供全双工的通信信道。

```js
var ws = new WebSocket('ws://host.com/path');

ws.onopen = () => {
  // 打开一个连接

  ws.send('something'); // 发送一个消息
};

ws.onmessage = (e) => {
  // 接收到了一个消息
  console.log(e.data);
};

ws.onerror = (e) => {
  // 发生了一个错误
  console.log(e.message);
};

ws.onclose = (e) => {
  // 连接被关闭了
  console.log(e.code, e.reason);
};
```

现在你的应用已经可以从各种渠道获取数据了，那么接下来面临的问题多半就是如何在不同的页面间组织和串联内容了。要管理页面的跳转，你需要学习[使用导航器跳转页面](navigation.html)。

### <十一> 其他参考资源

如果你耐心的读完并理解了本网站上的所有文档，那么你应该已经可以编写一个像样的React Native应用了。但是React Native并不全是某一家公司的作品——它汇聚了成千上万开源社区开发者的智慧结晶。如果你想深入研究React Native，那么建议不要错过下面这些参考资源。

## 常用的第三方库

如果你正在使用React Native，那你应该已经对[React](https://facebook.github.io/react/)有一定的了解了。React是基础中的基础所以我其实不太好意思提这个——但是，如果不幸你属于“但是”，那么请一定先了解下React，它也非常适合编写现代化的网站。

开发实践中的一个常见问题就是如何管理应用的“状态（state）”。这方面目前最流行的库非[Redux](http://redux.js.org/)莫属了。不要被Redux中经常出现的类似"reducer"这样的概念术语给吓住了——它其实是个很简单的库，网上也有很多优秀的[视频教程（英文）](https://egghead.io/courses/getting-started-with-redux) 。。

如果你在寻找具有某个特定功能的第三方库，那么可以看看别人[精心整理的资源列表](https://github.com/jondot/awesome-react-native)。这里还有个类似的[中文资源列表](https://github.com/reactnativecn/react-native-guide)。

## 示例应用

在[React Native Playground](https://rnplay.org/apps/picks)网站上有很多示例的代码。这个网站有个很酷的特性：它直接对接了真实设备，可以实时在网页上显示运行效果。当然，对于国内用户来说，可能访问很困难。

另外就是Facebook的F8开发大会有一个对应的app，这个app现在已经[开源](https://github.com/fbsamples/f8app)，其开发者还详细地撰写了[相关教程](http://f8-app.liaohuqiu.net/#content)。如果你想学习一个更实际更有深度的例子，那你应该看看这个。

## 开发工具

[Nuclide](https://nuclide.io/)是Facebook内部所使用的React Native开发工具。它最大的特点是自带调试功能，并且非常好地支持flow语法规则。（译注：然而我们还是推荐webstorm或是sublime text）。

[Ignite](https://github.com/infinitered/ignite)是一套整合了Redux以及一些常见UI组件的脚手架。它带有一个命令行可以生成app、组件或是容器。如果你喜欢它的选择搭配，那么不妨一试。

[CodePush](https://microsoft.github.io/code-push/)是由微软提供的热更新服务。热更新可以使你绕过AppStore的审核机制，直接修改已经上架的应用。对于国内用户，我们也推荐由本网站提供的[Pushy](http://update.reactnative.cn)热更新服务，相比CodePush来说，提供了全中文的文档和技术支持，服务器部署在国内速度更快，还提供了全自动的差量更新方式，大幅节约更新流量，欢迎朋友们试用和反馈意见！

[Exponent](http://docs.getexponent.com/versions/v6.0.0/index.html)是一套开发环境，还带有一个已上架的空应用容器。这样你可以在没有原生开发平台（Xcode或是Android Studio）的情况下直接编写React Native应用（当然这样你只能写js部分代码而没法写原生代码）。

[Deco](https://www.decosoftware.com/)是一个专为React Native设计的集成开发环境。它可以自动创建新项目、搜索开源组件并插入到项目中。你还可以实时地可视化地调整应用的界面。不过目前还只支持mac。

## React Native的交流社区

以下这些都是英文的交流区，我也就不翻译了……

The [React Native Community](https://www.facebook.com/groups/react.native.community) Facebook group has thousands of developers, and it's pretty active. Come there to show off your project, or ask how other people solved similar problems.

[Reactiflux](https://discord.gg/0ZcbPKXt5bZjGY5n) is a Discord chat where a lot of React-related discussion happens, including React Native. Discord is just like Slack except it works better for open source projects with a zillion contributors. Check out the #react-native channel.

The [React Twitter account](https://twitter.com/reactjs) covers both React and React Native. Following that account is a pretty good way to find out what's happening in the world of React.

There are a lot of [React Native Meetups](http://www.meetup.com/topics/react-native/) that happen around the world. Often there is React Native content in React meetups as well.

Sometimes we have React conferences. We posted the [videos from React.js Conf 2016](https://www.youtube.com/playlist?list=PLb0IAmt7-GS0M8Q95RIc2lOM6nc77q1IY), and we'll probably have more conferences in the future, too. Stay tuned.

欢迎朋友们在下方评论区分享中文教程和资源。
