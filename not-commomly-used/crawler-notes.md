# 爬虫注意点

- User-Agent
- Referer
- Cookie（爬取需要登录的网站）
- 爬取的频率，控制并发数（async module）
- IP（参考 [设置代理](https://github.com/hanzichi/funny-node/tree/master/proxy))
- JS 渲染后的页面
	- PhantomJS/CasPerJS（headless browser）
	- [phantomjs-node](https://github.com/amir20/phantomjs-node)（Node.js 模块，要先安装 phantomJS）
	- [nightmare](https://github.com/segmentio/nightmare)（A high-level browser automation library，可作为 Node.js 模块，这货有点大，100+M，集成了 [Electron](http://electron.atom.io/)）
