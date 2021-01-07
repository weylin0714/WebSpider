当我们在用 requests 抓取页面的时候，得到的结果可能会和在浏览器中看到的不一样：在浏览器中正常显示的页面数据，使用 requests 却没有得到结果。这是因为 requests 获取的都是原始 HTML 文档，而浏览器中的页面则是经过 JavaScript 数据处理后生成的结果。这些数据的来源有多种，可能是通过 Ajax 加载的，可能是包含在 HTML 文档中的，也可能是经过 JavaScript 和特定算法计算后生成的。

在本文我们就来了解什么是 Ajax 以及如何去分析和抓取 Ajax 请求。

## 1 Ajax 爬取原理

### 1.1 什么是 Ajax

Ajax，即异步的 JavaScript 和 XML，是利用 JavaScript 在保证页面不被刷新、页面链接不改变的情况下与服务器交换数据并更新部分网页的技术。

发送 Ajax 请求到网页更新的过程可以简单分为 3 步：

- 发送请求
- 解析内容
- 渲染网页

### 1.2 Ajax 分析

Ajax 有其特殊的请求类型，它叫作 xhr。可以利用 Chrome 开发者工具的筛选功能筛选出所有的 Ajax 请求。

## 2 Ajax 爬取案例实战

爬取网站：https://dynamic1.scrape.cuiqingcai.com/

