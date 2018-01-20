# PhantomJS 简单介绍

## 简介

[PhantomJS](http://phantomjs.org/) 是一个基于 WebKit 的服务器端 JavaScript API，它基于 BSD 开源协议发布。PhantomJS 无需浏览器的支持即可实现对 Web 的支持，且原生支持各种 Web 标准，如 DOM 处理、JavaScript、CSS 选择器、JSON、Canvas 和可缩放矢量图形 SVG。PhantomJS 主要是通过 JavaScript 和 CoffeeScript 控制 WebKit 的 CSS 选择器、可缩放矢量图形 SVG 和 HTTP 网络等各个模块，是一个 **无头浏览器**（headless browser）。而 [CasperJS](http://casperjs.org/) 封装了 PhantomJS 的 API，使其调用变的更加简单方便。（CasperJS 使得测试变的更加简单）


## PhantomJS 的使用场景如下：

- 无需浏览器的 Web 测试：无需浏览器的情况下进行快速的 Web 测试，且支持很多测试框架（浏览器自动化测试）。浏览器测试有别于 JS 代码的单元测试，后者一般是发布前的代码功能逻辑测试，在这方面已经有很多比较成熟的方案， [Jasmine](http://jasmine.github.io/2.3/introduction.html),  [mocha](https://github.com/mochajs/mocha),  [Qunit](http://qunitjs.com/) 等
- 页面自动化操作：使用标准的 DOM API 或一些 JavaScript 框架（如 jQuery）访问和操作 Web 页面 （表单提交，签到等，一系列需要登录才能完成的操作变的简单，不需要担心 Cookie 的问题）
- 屏幕捕获：以编程方式抓起 CSS、SVG 和 Canvas 等页面内容，即可实现网络爬虫应用。构建服务端 Web 图形应用，如截图服务、矢量光栅图应用 
- 网络监控：自动进行网络性能监控、跟踪页面加载情况以及将相关监控的信息以标准的 HAR 格式导出（截图，像素级的对比，从而发现页面异常；加载速度的监控，预警）
- 爬虫：个人恶趣味，能够爬取 JavaScript 渲染后的网站代码（ajax 爬虫），同时使得需要登录才能完成的一些操作变的简单


## 局限性：

- 遇到验证码的情况，一般可能在登录过程中或者提交表单后
- PhantomJS/CasperJS 提供的是一个无界面的 Webkit 内核浏览器，**所以无法覆盖 IE 浏览器**，目前 Gecko 内核的无界面浏览器已经有解决方案 [SlimerJS](http://slimerjs.org/)，并且支持与 PhantomJS 一模一样的 API


## Read more:

- [PhantomJS - 阮一峰](http://javascript.ruanyifeng.com/tool/phantomjs.html)
- [End To End Testing with PhantomJS and CasperJS](http://thejsguy.com/2015/02/28/end-to-end-testing-with-phantomsjs-and-casperjs.htm)