### wx.connectSocket(OBJECT)

创建一个 WebSocket 连接；一个微信小程序同时只能有一个 WebSocket 连接，如果当前已存在一个 WebSocket 连接，会自动关闭该连接，并重新创建一个 WebSocket 连接。

OBJECT参数说明：

![](http://images2015.cnblogs.com/blog/602490/201611/602490-20161116161954404-171311682.png)

示例代码：



```
wx.connectSocket({
  url: 'test.php',
  data:{
    x: '',
    y: ''  },
  header:{ 
    'content-type': 'application/json'  },
  method:"GET"})
```



### wx.onSocketOpen(CALLBACK)

监听WebSocket连接打开事件。

示例代码：

```
wx.connectSocket({
  url: 'test.php'})
wx.onSocketOpen(function(res) {
  console.log('WebSocket连接已打开！')
})
```

### wx.onSocketError(CALLBACK)

监听WebSocket错误。

示例代码：



```
wx.connectSocket({
  url: 'test.php'})
wx.onSocketOpen(function(res){
  console.log('WebSocket连接已打开！')
})
wx.onSocketError(function(res){
  console.log('WebSocket连接打开失败，请检查！')
})
```



### wx.sendSocketMessage(OBJECT)

通过 WebSocket 连接发送数据，需要先 wx.connectSocket，并在 wx.onSocketOpen 回调之后才能发送。

OBJECT参数说明：

![](http://images2015.cnblogs.com/blog/602490/201611/602490-20161116162408638-1485655567.png)

示例代码：



```
var socketOpen = false
var socketMsgQueue = []
wx.connectSocket({
  url: 'test.php'})

wx.onSocketOpen(function(res) {
  socketOpen = true
  for (var i = 0; i < socketMsgQueue.length; i++){
     sendSocketMessage(socketMsgQueue[i])
  }
  socketMsgQueue = []
})

function sendSocketMessage(msg) {
  if (socketOpen) {
    wx.sendSocketMessage({
      data:msg
    })
  } else {
     socketMsgQueue.push(msg)
  }
}
```



### wx.onSocketMessage(CALLBACK)

监听WebSocket接受到服务器的消息事件。

CALLBACK返回参数：

![](http://images2015.cnblogs.com/blog/602490/201611/602490-20161116162559107-655470719.png)

示例代码：



```
wx.connectSocket({
  url: 'test.php'})

wx.onSocketMessage(function(res) {
  console.log('收到服务器内容：' + res.data)
})
```



### wx.closeSocket()

关闭WebSocket连接。

### wx.onSocketClose(CALLBACK)

监听WebSocket关闭。



```
wx.connectSocket({
  url: 'test.php'})//注意这里有时序问题，//如果 wx.connectSocket 还没回调 wx.onSocketOpen，而先调用 wx.closeSocket，那么就做不到关闭 WebSocket 的目的。//必须在 WebSocket 打开期间调用 wx.closeSocket 才能关闭。
wx.onSocketOpen(function() {
  wx.closeSocket()
})

wx.onSocketClose(function(res) {
  console.log('WebSocket 已关闭！')
})
```

