## HTML5离线存储

manifest文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容），支持manifest的浏览器，会将按照manifest文件的规则，将文件保存在本地，从而在没有网络链接的情况下，也能访问页面。

### html中使用方式
```
<html manifest="demo.appcache">

<body>
The content of the document......
</body>

</html>
```
### manifest文件的编写规范

```
CACHE MANIFEST
# 注释：需要缓存的文件，无论在线与否，均从缓存里读取
CACHE:
chched.js
cached.css

# 注释：不缓存的文件，无论缓存中存在与否，均从新获取
NETWORK:
uncached.js
uncached.css

# 注释：获取不到资源时的备选路径，如index.html访问失败，则返回404页面
FALLBACK:
index.html 404.html
```



## localstorge cookie sessionstorge 区别
  1.localstorge浏览器本地存储单页面关闭时也不会被销毁
  2.sessionstorge只在页面会话期有效页面关闭时数据销毁
  3.cookie关闭时也不会被销毁但是内容存储一般为4kb左右一般存用户的登录信息

## 页面可见性Api
  当用户最小化窗口或切换到另一个选项卡时，API会发送visibilitychange事件，让听众知道页面状态已更改。你可以检测事件并执行某些操作或行为不同。例如，如果您的网络应用正在播放视频，则可以在用户将标签放入背景时暂停视频，并在用户返回标签时恢复播放。 用户不会在视频中丢失位置，视频的音轨不会干扰新前景选项卡中的音频，并且用户在此期间不会错过任何视频
## css优先级算法
  行内样式 > 嵌入样式表 > 外链样式表
  ！important > id > class > tag
  ！import权重最高
## 请解释一下为什么需要清除浮动？清除浮动的方式
  清除浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷使我们页面后面的布局不能正常显示。
  1.clear:both
  2.浮动元素的父级div定义伪类:after

## [VUE.js的双向绑定原理](https://juejin.im/entry/5923973da22b9d005893805a)
  通过数据劫持结合发布者-订阅者模式的方式来实现的双向绑定  
  在getter中初始化需要被订阅的对象  
  在setter中如果数据变化，通知所有订阅者
  数据劫持Object.defineProperty()方法  
  Object.defineProperty(obj, prop, descriptor)：直接在一个对象上定义一个新属性，或者修改一个新属性，并返回这个对象  

  1.通过实现一个observer，实质是Object.defineProperty()方法设置get、set的来监听属性的变化，dep.notify()通知dep订阅器监听vue实例中的所有属性变化      

  2.消息订阅器Dep容器，订阅器Dep主要负责收集订阅者(收集被监听的对象)(ps：Dep=[watcher1,watcher2,watcher3.....]) 

  3.订阅者Watcher在初始化的时候需要将自己添加进订阅器Dep中（只要在订阅者Watcher初始化的时候才需要添加订阅者到dep中）  
  流程当observer监听值发生改变时通知Dep订阅容器，在遍历Dep所有订阅者，然后触发Dep原型上的notify方法通知wather中判断新值与老值是否有变化  

  4.Compile编译解析Dom  
      1.解析模板指令，并替换模板数据，初始化视图
      2.将模板指令对应的节点绑定对应的更新函数，初始化相应的订阅器
  ![GitHub](https://user-gold-cdn.xitu.io/2017/5/23/04fdcd64ed41f762a7a495f73c0a2f86?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)