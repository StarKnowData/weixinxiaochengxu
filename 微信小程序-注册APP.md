### App()

`App()` 函数用来注册一个小程序。接受一个 object 参数，其指定小程序的生命周期函数等。

object参数说明

onLaunch   　　　　Function 　　　　生命周期函数--监听小程序初始化 　　　　当小程序初始化完成时，会触发 onLaunch（全局只触发一次）  
onShow  　　　　　 Function  　　　　生命周期函数--监听小程序显示 　　　　   当小程序启动，或从后台进入前台显示，会触发 onShow  
onHide  　　　　　　Function  　　　　生命周期函数--监听小程序隐藏 　　　　　　当小程序从前台进入后台，会触发 onHide  
其他  　　　　　　　　Any 　　　　　　开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问

前台、后台定义： 当用户点击左上角关闭，或者按了设备 Home 键离开微信，小程序并没有直接销毁，而是进入了后台；当再次进入微信或再次打开小程序，又会从后台进入前台。

只有当小程序进入后台一定时间，或者系统资源占用过高，才会被真正的销毁。

示例代码：

```js
App({
  onLaunch: function() { 
    // Do something initial when launch.
  },
  onShow: function() {
      // Do something when show.
  },
  onHide: function() {
      // Do something when hide.
  },
  globalData: 'I am global data'
})程序运行过程
```

![](http://images2015.cnblogs.com/blog/602490/201611/602490-20161109165152139-1240067184.png)

```js

```

getCurrentPages()

`getCurrentPages()` 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。

初始化 　　　　　　新页面入栈  
打开新页面 　　　　新页面入栈  
页面重定向 　　　　当前页面出栈，新页面入栈  
页面返回  　　　　　页面不断出栈，直到目标返回页，新页面入栈  
Tab切换　　 　　　当前页面出栈，新页面入栈

### getApp()

我们提供了全局的 `getApp()` 函数，可以获取到小程序实例。

```javascript
// other.js
var appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```

注意：

`App()` 必须在 `app.js` 中注册，且不能注册多个。

不要在定义于 `App()` 内的函数中调用 `getApp()` ，使用 `this` 就可以拿到 app 实例。

不要在 onLaunch 的时候调用 `getCurrentPage()`，此时 page 还没有生成。

通过 `getApp()` 获取实例之后，不要私自调用生命周期函数。