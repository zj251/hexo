---
title: iframe + postMessage 跨域通信
---
>Web是构建在同源策略基础之上，同源策略是浏览器的行为，是为了保护本地数据不被JavaScript代码获取回来的数据污染，因此拦截的是客户端发出的请求回来的数据接收，即请求发送了，服务器响应了，但是无法被浏览器接收。我们可以通过iframe来实现不同域之间互相请求资源。

## 同源

- 概念：协议相同、域名相同、端口相同
- 目的：保证用户信息的安全，防止用户数据泄露
- 限制范围：
  - Cookie、LocalStorage、 IndexDB 无法读取
  - Dom 无法获取
  - AJAX 请求在浏览器端有跨域限制
  
## iframe
### 基本属性
- src iframe地址
- name iframe名称
- width
- height
- frameborder 否显示框架周围的边框 
	- 1 有边框
    - 0 无边框
- scrolling 是否在 iframe 中显示滚动条
	- yes 
    - no
    - auto

## postMessage
postMessage是HTML5中新引入的API，该方法可以通过绑定window的message事件来实现跨窗口以及跨域的通信。

### 应用场景
- 页面和其打开的新窗口的数据传递
- 页面与嵌套的 iframe 进行消息传递
- 多窗口之间消息传递


## iframe + postMessage 使用
```
父窗口（react页面）

export default class Demo {
   constructor() {
      this.iframeUrl = "https://www.a.com"
   }
   
   componentDidMount() {
        // 监听message事件
        window.addEventListener("message", this.receiveMessage, false);
    }
    
    /* 事件处理函数 */
    receiveMessage =  ( event ) => {
        const {type, data} = event.data;
        console.log("从son拿到了type" + type + "以及data" + "data"了呀);
    }
   
   /* 发送消息 */
   sendMessage = () => {
       // 为了security，当使用 postMessage 将数据发送到其他窗口时，始终指定精确的目标，而不是 *
       window.frames[0].postMessage({type: 'sendMessageToSon'}, 'https://www.b.com');
   }

   render(){
       return <div>
    		 <iframe src={this.iframeUrl}  frameBorder="0" width="800px" height="90%"/>
                 <button onClick={this.sendMessage}>发送消息到父窗口</button>
    	      </div>
   }
}
  
```
```
子窗口（iframe页面）

<script>
  // 监听message事件，接收消息
  window.addEventListener("message", receiveFatherMessage, false);

  // 添加事件处理函数
  function receiveFatherMessage ( event ) {
  
     // 为了security，如果希望从其他网站接收message，请始终使用 origin 和 source 属性验证发件人的身份
     if(event.origin !== 'https://www.baidu.com')
     return;
     
     console.log( '我是son,我接收到了：', event.data );
  }
  
  sendMessage() {
     const value =  document.getElementById("message").value;
     window.parent.postMessage(value, "*");
  }
  
</script>
<body>
  <input type="text" id="message"/>
  <button onclick="sendMessage()">发送消息到子窗口</button>
</body>


```



