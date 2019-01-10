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

  4.Compile编译解析Dom  
      1.解析模板指令，并替换模板数据，初始化视图
      2.将模板指令对应的节点绑定对应的更新函数，初始化相应的订阅器  

#### 双向绑定流程：
  ``` 
    compile编译解析DOM节点 遍历节点匹配v-model指令绑定更新函数（callback（）就是innerHTML）找到订阅者，
    --->然后watcher方法中向dep添加watcher
    compile-->初始化视图层
    首先Observer监听所有属性，当通过视图层（如：input改变绑定属性值）或模型层改变某个属性的时候，通知----->Dep(订阅器)  
    --->遍历Dep触发每个watcher(订阅者）的updatpe方法比较oldVal！== newVal 当不相等时说明这个属性已经改变了
    --->然后通过传入watcher的callback（dom.innerHTML）来渲染页面  
    
  ```
  <img src="https://user-gold-cdn.xitu.io/2017/5/23/04fdcd64ed41f762a7a495f73c0a2f86?imageView2/0/w/1280/h/960/format/webp/ignore-error/1"/>

## get 与 post 的区别
  1.get:把参数包含在URL中。	 
  2.post：参数通过request-body传递参数，没有参数长度限制  
  3.get请求参数有长度限制而post没有长度限制  
  4.参数类型:get请求只接受ASCII字符，而post没有限制  
  5.**GET请求只能进行url（encodeURIComponent）编码，而POST支持多种编码方式**

## Xss 与 Csrf 网络安全知识
  Xss:**跨站脚本攻击**,  脚本注入  
  >  原理:攻击者的输入没有经过后台的过滤直接进入到数据库，最终显示给来访的用户。  
  防御方法：避免 XSS 的方法之一主要是对**用户输入的内容进行过滤**  

  CSRF:**跨站请求伪造**
  >  原理:受害者登录受信任网站A，并在本地生成Cookie，然后在访问危险网站B。  
  防御：给每个表单加入随机 Token 进行验证，这样B页面无法获取A页面的 Token 导致请求验证失败，从而防止了 CSRF。  

## [post 请求中 Content-Type](https://imququ.com/post/four-ways-to-post-data-in-http.html)    
  Content-Type 字段来获知请求中的消息主体是用何种方式编码，再对主体进行解析  
  > application/x-www-form-urlencoded 

  >application/json

  >text/xml

  >multipart/form-data  

## 请求状态码
  * 200 请求成功
  * 301 请求资源已被永久的移到了新URL
  * 302 请求资源临时转移到新URl
  * 304 请求的资源未修改
  * 401 请求要求用户身份认证
  * 403 服务器拒绝执行请求
  * 404 请求地址不存在
  * 500 服务器错误
  * 501 服务器不支持请求的功能
