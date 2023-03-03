## 简介

| 类型            | 简介                                       | 调试                                       | 通信                                       |
| ------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| content.js    | 能作用于页面，能获取页面的 dom；开辟了独立空间，所以不能获取原有页面上的全局变量（比如 $），不能调用原有页面上的方法；不能跨域请求（经测试，可 get 不和 post 跨域，待确认） | 和原页面共享调试窗口（打开控制台，然后Sources->Content Scripts，Console 里也可以切换） | 能和 background.js/popup.js 通信             |
| injected.js   | 能作用于页面，获取页面的 dom；**能获取页面上的全局变量，调用原有页面的方法**；不能跨域请求。 | 用类似 $.getScript() 的方式将 js 插入页面，和原页面的 js 并没有什么区别。和原页面共享调试窗口 | 不能和 background.js/popup.js 通信。只能和 content.js 通信（通过 postMessage API） |
| background.js | 脚本一直在后台运行。不能获取页面 dom、全局变量。**可以跨域请求**（v2ex-helper 微博图床功能） | 调试打印在背景页。具体打开方式，先在浏览器地址栏输入 <chrome://extensions/>，然后找到对应的扩展，单击「背景页」 | 能和 content.js 通信；能和 popup.js 互相调用方法（通过 ` chrome.extension.getViews({type: 'popup'})` 获取 views 数组，**此时 popup.html 页面必须打开才能被获取到**） |
| popup.js      | 和 background.js 本质类似，且能互相调用方法，当然也能跨域请求。只能获取 option.html 的 dom，并且该 dom 只能被 option.js 获取 | 右键浏览器右上角的扩展图标->审查弹出内容；或者点击浏览器右上角的扩展图标，弹出 popup.html 页面，然后右键->检查 | 能和 content.js 通信；能和 background.js 互相调用方法，通过`chrome.extension.getBackgroundPage()` 能获取 background 的 window 对象 |
| option.js     | 只能获取 option.html 的 dom，并且该 dom 只能被 option.js 获取 | 右键浏览器右上角的扩展图标->选项，弹出 option.html 页面，然后右键->检查 | 能和 background.js/popup.js 通信             |

## 速记

background.js 控制一切，将扩展的主要逻辑都放在 background 中比较便于管理，其他页面通过消息传递机制和 background 通信。理论上 content.js 也能直接和 popup.js 通信，但是不建议，建议用 background 做中介。

### content（主动）向 background 通信：

```js
// content.js
chrome.runtime.sendMessage({
  msg: '这是从 content.js 发来的消息'
}, response => {
  if (response) {
    console.log(response.msg)
  }
})
```

```js
// background.js
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  console.log(message.msg)

  sendResponse({msg: '这是从 background 发回的消息'})
})
```

### background/popup（主动）向 content 通信：

```js
// background.js/popup.js
chrome.tabs.query({active: true, currentWindow: true}, function(tabs) {
  chrome.tabs.sendMessage(tabs[0].id, {msg: '这是从 background 发来的消息'}, function(response) {
    if (response) {
      console.log(response.msg)
    }
  })
})  
```

```js
// content.js
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  console.log(message.msg)

  sendResponse({msg: '这是从 content 发回的消息'})
})
```

一般开发中可能会有 popup 向 content 通信的需求，注意这个时候 popup.html 页面打开 popup.js 才能生效，如果场景复杂的话建议用 background 做中介。

### injected.js 的插入

injected.js 通常用于获取原页面的全局变量、方法。

如果是简单的 js，可以直接用类似 $.getScript 的方法插入：

```js
var script = document.createElement('script')
script.text = `console.log('this is injected.js')`
script.onload = function() {
  this.parentNode.removeChild(this);
};
document.body.appendChild(script)
```

如果是复杂点 js，写到一个独立的文件比较好：

```js
var s = document.createElement('script')
s.src = chrome.extension.getURL('injected-scripts/injected.js')
s.onload = function() {
  this.parentNode.removeChild(this)
}
document.body.appendChild(s)
```

在 manifest.json 文件中配置如下：

```json
"web_accessible_resources": ["injected-scripts/injected.js"]
```

## 其他

一些其他值得注意的点：

- js 均需要外链引入，不能 inline
- js 不能用线上 CDN 引入
- content.js 可跨域。个人测试如下（不确定），如果是 get 跨域，content.js 需要在 "permissions" 中将跨域地址声明后可跨域请求，如果是 post 则失败。background.js 如果做 get 请求，无需 "permissions" 声明，如果是 post 则需要 "permissions" 声明

## 参考链接

- [官网](https://developer.chrome.com/extensions)
- [扩展发布链接](https://chrome.google.com/webstore/developer/dashboard)
- [Chrome扩展及应用开发（图灵社区）](http://www.ituring.com.cn/book/1421)
- [一篇非常不错的入门文章](http://www.cnblogs.com/liuxianan/p/chrome-plugin-develop.html)