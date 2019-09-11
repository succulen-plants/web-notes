#### React避免重新渲染

#### shouldComponentUpdate ， 该生命周期函数用来控制组件是否应该被更新

```
shouldComponentUpdate(nextProps, nextState)
这个生命周期函数返回一个布尔值，如果返回true，那么当props或state改变的时候进行更新；如果返回false，当props或state改变的时候不更新，默认返回true

```

​	组件可以通过重写这个生命周期函数shouldComponentUpdate来提升速度：

```
shouldComponentUpdate(nextProps, nextState) {
  if (this.props.color !== nextProps.color) {
   return true;
  }
  if (this.state.count !== nextState.count) {
   return true;
  }
  return false;
 }
 shouldComponentUpdate只检查props.color和state.count的变化。如果这些值没有变化，组件就不会更新。当你的组件变得更加复杂时，你可以使用类似的模式来做一个“浅比较”，用来比较属性和值以判定是否需要更新组件

```



#### React避免重新渲染

#### shouldComponentUpdate ， 该生命周期函数用来控制组件是否应该被更新

```
shouldComponentUpdate(nextProps, nextState)
这个生命周期函数返回一个布尔值，如果返回true，那么当props或state改变的时候进行更新；如果返回false，当props或state改变的时候不更新，默认返回true

```

​	组件可以通过重写这个生命周期函数shouldComponentUpdate来提升速度：

```
shouldComponentUpdate(nextProps, nextState) {
  if (this.props.color !== nextProps.color) {
   return true;
  }
  if (this.state.count !== nextState.count) {
   return true;
  }
  return false;
 }
 shouldComponentUpdate只检查props.color和state.count的变化。如果这些值没有变化，组件就不会更新。当你的组件变得更加复杂时，你可以使用类似的模式来做一个“浅比较”，用来比较属性和值以判定是否需要更新组件

```

