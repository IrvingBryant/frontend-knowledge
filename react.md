## [ Portals ] (https://www.reactjscn.com/docs/portals.html)
  语法：
  ```
    ReactDOM.createPortal(child, container)
  ```
  第一个参数（child）是任何可渲染的 React 子元素，例如一个元素，字符串或碎片。第二个参数（container）则是一个 DOM 元素。
  用法：
    一般来说当从组建的render()方法返回的一个元素，该元素仅能装配 DOM 节点中离其最近的父元素：
    ```
      render() {
        // React mounts a new div and renders the children into it
        return (
          <div>
            {this.props.children}
          </div>
        );
      }
    ```
    然而，有时候将其插入到 DOM 节点的不同位置也是有用的：
    ```
      render() {
      // React does *not* create a new div. It renders the children into `domNode`.
      // `domNode` is any valid DOM node, regardless of its location in the DOM.
      return ReactDOM.createPortal(
        this.props.children,
        domNode,
      );
    }
    ```

## [错误边界](https://www.reactjscn.com/docs/error-boundaries.html) 
  错误边界的用途：类似try{}catch{} 不影响后面代码执行
  部分 UI 的异常不应该破坏了整个应用。为了解决 React 用户的这一问题，React 16 引入了一种称为 “错误边界” 的新概念。

  错误边界是用于捕获其子组件树 JavaScript 异常，记录错误并展示一个回退的 UI 的 React 组件，而不是整个组件树的异常。错误组件在渲染期间，生命周期方法内，以及整个组件树构造函数内捕获错误。

  一个类组建定义了componentDidCatch(error,info)的新方法，则其成为一个错误边界
    ```
      componentDidCatch(error, info) {
        // Display fallback UI
        this.setState({ hasError: true });
        // You can also log the error to an error reporting service
        logErrorToMyService(error, info);
      }
    ```
    componentDidCatch() 方法机制类似于 JavaScript catch {}，但是针对组件
  注意错误边界仅可以捕获其子组件的错误。错误边界无法捕获其自身的错误。如果一个错误边界无法渲染错误信息，则错误会向上冒泡至最接近的错误边界。这也类似于 JavaScript 中 catch {} 的工作机制