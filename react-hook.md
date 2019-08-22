## react hook v16.8.6

###  `state hook ---- useState`

  `useState hook 可以让没有状态的函数组件拥有class组件的 state`
```
  import React ,{useState} from 'react
  
  function A(){
    //用了数组解构的语法 
    //[name,setName] = ['sheyang',setName(){}]
    //[a, b] = [1, 2]; a = 1 、 b = 2
    //useState('sheyang') = ['sheyang',setName(){}]

     
    // var fruitStateVariable = useState('banana'); // 返回一个有两个元素的数组
    // var fruit = fruitStateVariable[0]; // 数组里的第一个值
    // var setFruit = fruitStateVariable[1]; // 数组里的第二个值
    const [name,setName] = useState('sheyang');
    return <div onClick={()=>{
      setName('哈哈')
    }}>{name}</div>
  }
  //上面等价于
  class A extends React.compontent{
    constructor(props){
      super(props)
      this.state={
        name:'sheyang'
    }
    render(){
      return {
        <div onClick={()=>{
          this.setState({
            name:'哈哈'
          })
        }}>{this.state.name}</div>
      }
    }
  }
  
```
// 声明多个 state 变量  
  const [age, setAge] = useState(42);  

  const [fruit, setFruit] = useState('banana');  

  const [todos, setTodos] = useState([{ text: '学习 Hook' }]);

### `effect hook ---- useEffect`

  `Effect Hook 可以让你在函数组件中执行副作用（函数式编程中概念）操作 类似class组件内部的生命周期`
  ```
    import React, { useState, useEffect } from 'react';
    function Example() {
      const [count, setCount] = useState(0);

      // Similar to componentDidMount and componentDidUpdate:
      useEffect(() => {
        // Update the document title using the browser API
        document.title = `You clicked ${count} times`;
      });
      /*
      useEffect(()=>{
        document.title = ` ABC`
      })
      */

      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
            Click me
          </button>
        </div>
      );
    }
    //等价于
    class Example extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          count: 0
        };
      }

      componentDidMount() {
        document.title = `You clicked ${this.state.count} times`;
      }

      componentDidUpdate() {
        document.title = `You clicked ${this.state.count} times`;
      }

      render() {
        return (
          <div>
            <p>You clicked {this.state.count} times</p>
            <button onClick={() => this.setState({ count: this.state.count + 1 })}>
              Click me
            </button>
          </div>
        );
      }
    }
  ```  
  `useEffect Hook 它在第一次渲染之后和每次更新之后都会执行。useEffect接收副作用函数 `
  `提示如果你熟悉 React class 的生命周期函数，你可以把 useEffect Hook 看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。`

   #### `react 中有两种常见副作用：需要清除和不需要清除`
   * 不需要清除
   > 有时候，我们只想在 React 更新 DOM 之后运行一些额外的代码。比如发送网络请求，手动变更 DOM，记录日志，这些都是常见的无需清除的操作。
   * 需要清除 
   > 定时器 、 监听订阅与取消订阅

  需要清除的副作用 类比写法
  ```
    import React, { useState, useEffect } from 'react';

    function FriendStatus(props) {
      const [isOnline, setIsOnline] = useState(null);
      //了解了 useEffect 可以在组件渲染后实现各种不同的副作用。有些副作用可能需要清除，所以需要返回一个函数：
      useEffect(() => {
        function handleStatusChange(status) {
          setIsOnline(status.isOnline);
        }
        
        ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
        // Specify how to clean up after this effect:
        return function cleanup() {
          ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
        };
      });

      if (isOnline === null) {
        return 'Loading...';
      }
      return isOnline ? 'Online' : 'Offline';
    }
    //等价于
      class FriendStatus extends React.Component {
        constructor(props) {
          super(props);
          this.state = { isOnline: null };
          this.handleStatusChange = this.handleStatusChange.bind(this);
        }

        componentDidMount() {
          ChatAPI.subscribeToFriendStatus(
            this.props.friend.id,
            this.handleStatusChange
          );
        }

        componentWillUnmount() {
          ChatAPI.unsubscribeFromFriendStatus(
            this.props.friend.id,
            this.handleStatusChange
          );
        }

        handleStatusChange(status) {
          this.setState({
            isOnline: status.isOnline
          });
        }

        render() {
          if (this.state.isOnline === null) {
            return 'Loading...';
          }
          return this.state.isOnline ? 'Online' : 'Offline';
        }
      }
    
  ```
  `了解了 useEffect 可以在组件渲染后实现各种不同的副作用。有些副作用可能需要清除，所以需要返回一个函数：`  

  `使用多个 Effect 实现关注点分离`  

  `忘记正确地处理 componentDidUpdate 是 React 应用中常见的 bug 来源。`  

  #### `通过跳过 Effect 进行性能优化`
  ```
  componentDidUpdate(prevProps, prevState) {
    //上一个状态和当前状态不相等时才更新
    if (prevState.count !== this.state.count) {
      document.title = `You clicked ${this.state.count} times`;
    }
  }
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); // 仅在 count 更改时更新
  /*
    上面这个示例中，我们传入 [count] 作为第二个参数。这个参数是什么作用呢？如果 count 的值是 5，而且我们的组件重渲染的时候 count 还是等于 5，React 将对前一次渲染的 [5] 和后一次渲染的 [5] 进行比较。因为数组中的所有元素都是相等的(5 === 5)，React 会跳过这个 effect，这就实现了性能的优化。

    当渲染时，如果 count 的值更新成了 6，React 将会把前一次渲染时的数组 [5] 和这次渲染的数组 [6] 中的元素进行对比。这次因为 5 !== 6，React 就会再次调用 effect。如果数组中有多个元素，即使只有一个元素发生变化，React 也会执行 effect。 
    
  */


  //对于清除副作用的effect同样有效
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  }, [props.friend.id]); // 仅在 props.friend.id 发生变化时，重新订阅
  ```

  
