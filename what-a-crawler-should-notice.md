# 爬虫注意点

- User-Agent
- Referer
- Cookie（爬取需要登录的网站）
- 爬取的频率，控制并发数 （async module）
- IP
- JS 渲染后的页面
	- [PhantomJS/CasPerJS](https://github.com/hanzichi/personal-collections/blob/master/phantomjs-casperjs-introduction.md)（headless browser）
	- [phantomjs-node](https://github.com/amir20/phantomjs-node)（Node.js 模块，要先安装 phantomJS）
	- [nightmare](https://github.com/segmentio/nightmare)（A high-level browser automation library，可作为 Node.js 模块，这货有点大，50+M，集成了 Electron）