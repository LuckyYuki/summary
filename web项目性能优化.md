# web项目性能优化

减少页面体积，提升网络加载：

1）静态资源压缩合并，如js代码合并，css代码合并，压缩图片等；

2）静态资源缓存；

3）使用 CDN 让资源加载更快；

优化页面渲染：

1）减少HTTP请求；

2）减少DOM操作，多个操作尽量合并在一起执行（DocumentFragment）；

3）懒加载（图片懒加载、下拉加载更多）；

4）使用 SSR 后端渲染，数据直接输出到 HTML 中，减少浏览器使用 JS 模板渲染页面 HTML 的时间（smarty、Vue SSR）；
