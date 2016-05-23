
---
layout:     post
title:      ES6 in React/React-Native
date: 2016-05-09
tags: JavaScript
description: 
---

## module

加载模块

```js
//es5
var React = require("react-native")
var {
    Image,
    Text,
    PropTypes
} = React  //引用不同的React Native组件
```
```js
//es6
import React, {
    Image, 
    Text,
    PropTypes
} from 'react-native'
```

```js
// Reacr-Native 0.25+

import React from 'react'
import { Image, Text, PropTypes } from 'react-native'
```
导出模块

```js
//es5
var MyComponent = React.createClass({
    ...
});
module.exports = MyComponent;
```

```js
//es6
export default class MyComponent extends React.Component{
    ...
}
```
## Class

#### React.createClass

```js
// es5
var Photo = React.createClass({
  handleDoubleTap: function(e) { … },
  render: function() { … },
});
```

```js
// es6+ 
class Photo extends React.Component {
  handleDoubleTap(e) { … }
  render() { … }
}
```

#### componentWillMount

```js
// es5
var EmbedModal = React.createClass({
  componentWillMount: function() { … },
});
```

```js
// es6+
class EmbedModal extends React.Component {
  constructor(props) {
    super(props);
    // Operations usually carried out in componentWillMount go here
  }
}
```

## Property initializers

```js
// es5
var Video = React.createClass({
  getDefaultProps: function() {
    return {
      autoPlay: false,
      maxLoops: 10,
    };
  },
  getInitialState: function() {
    return {
      loopsRemaining: this.props.maxLoops,
    };
  },
  propTypes: {
    autoPlay: React.PropTypes.bool.isRequired,
    maxLoops: React.PropTypes.number.isRequired,
    posterFrameSrc: React.PropTypes.string.isRequired,
    videoSrc: React.PropTypes.string.isRequired,
  },
});
```

```js
// es6=>react-native
class Video extends React.Component {
  static defaultProps = {
    autoPlay: false,
    maxLoops: 10,
  }
  static propTypes = {
    autoPlay: React.PropTypes.bool.isRequired,
    maxLoops: React.PropTypes.number.isRequired,
    posterFrameSrc: React.PropTypes.string.isRequired,
    videoSrc: React.PropTypes.string.isRequired,
  }
  
}
```
因为 `static` 在ie 10 以及之前的版本兼容问题， Airbnb 的 react 写法是下面这样的，不过react-native不用考虑这个问题。

```js
// es6 => react
const defaultProps = {
  autoPlay: false,
  maxLoops: 10,
}
const propTypes = {
  autoPlay: React.PropTypes.bool.isRequired,
  maxLoops: React.PropTypes.number.isRequired,
  posterFrameSrc: React.PropTypes.string.isRequired,
  videoSrc: React.PropTypes.string.isRequired,
}
class Video extends React.Component {
  
}
Video.propTypes = propTypes
Video.defaultProps = defaultProps
```

## 箭头函数

```js
// es5
var PostInfo = React.createClass({
  handleOptionsButtonClick: function(e) {
    // Here, 'this' refers to the component instance.
    this.setState({showOptionsModal: true});
  },
});
```

```js
// es6
class PostInfo extends React.Component {
  handleOptionsButtonClick = (e) => {
    this.setState({showOptionsModal: true});
  }
}
```

## 字符串模板

```js
// es5
var Form = React.createClass({
  onChange: function(inputName, e) {
    var stateToSet = {};
    stateToSet[inputName + 'Value'] = e.target.value;
    this.setState(stateToSet);
  },
});
```

```js
// es6
class Form extends React.Component {
  onChange(inputName, e) {
    this.setState({
      [`${inputName}Value`]: e.target.value,
    });
  }
}
```

## initialState

初始化 state 的时候，以往是这么写的：

```js
// es5
var Video = React.createClass({
  getInitialState: function() {
      return {
          loopsRemaining: this.props.maxLoops,
      };
  },
})
```

```js
//es6
class Video extends React.Component {
  state = {
    loopsRemaining: this.props.maxLoops,
  }
}
```
es6 里还一种写法：

```js
//es6
class Video extends React.Component {
  constructor(props){
    super(props);
    this.state = {
      loopsRemaining: this.props.maxLoops,
    };
  }
}
```

## 解构和传播属性

结合使用es6+的解构和属性传播，我们给子组件传递一批属性更为方便了。这个例子 把className 以外的所有属性传递给 div 标签：

```js
// es6
class AutoloadingPostsGrid extends React.Component {
  render() {
    var {
      className,
      ...others,  // contains all properties of this.props except for className
    } = this.props;
    return (
      <div className={className}>
          <PostsGrid {...others} />
          <button onClick={this.handleLoadMoreClick}>Load more</button>
      </div>
    );
  }
}
```

下面这种写法，则是传递所有属性的同时，用覆盖新的 className 值：

```js
<div {...this.props} className="override">
    …
</div>
```

这个例子则相反，如果属性中没有包含 className，则提供默认的值，而如果属性中已经包含了，则使用属性中的值

```js
<div className="base" {...this.props}>
    …
</div>
```

参考：https://babeljs.io/blog/2015/06/07/react-on-es6-plus