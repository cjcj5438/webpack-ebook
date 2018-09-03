# 使用middleware搭建


- Express or Koa 
- webpack-dev-middleware 模块服务
- webpack-hot-middleware 模块热更新
- http-proxy-middleware 帮助做代理的 
- connect-history-api-fallback 做地址重定向的
- opn 自动代开浏览器

```npm
npm install express opn webpack-dev-middleware webpack-hot-middleware http-proxy-middleware connect-history-api-fallback --save-dev
```

> 首先是基于上章节的配置来开发的

```javascript 1.8
//server.js
const express = require("express");
const webpack = require("webpack");
const opn = require("opn");
const webpackDevMiddleware = require('webpack-dev-middleware');
const webpackHotMiddleware = require("webpack-hot-middleware");
const proxyMiddleware = require("http-proxy-middleware");
const historyApiFallback = require("connect-history-api-fallback");

const app = express();
const port = 3000;

const proxyTable=require('./proxy');

//拿到webpack处理的配置  > compiler
const config=require('./webpack.common.conf')("development");
const compiler=webpack(config);
// 那怎么用呢? 挂到app.use();
app.use(webpackDevMiddleware(compiler,{
  publickPath:config.output.publicPath
}));
app.use(webpackHotMiddleware(compiler));

// 现在使用proxy 和historapifallback
// 首先呢先把 devServer:{proxy 和 historyapifallbact} 中的配置先提取出来
for(let context in proxyTable){
  app.use(proxyMiddleware((context,proxyTable[context])));
}
app.use(historyApiFallback(require('./historyfallback')));


app.listen(port, function () {
  console.log("sucess listen to " + port);
  opn("http://localhost:" + port);
});

```

```javascript 1.8
// proxy.js
module.exports={
  '/.+': {
    target: 'https://m.weibo.cn',
    changeOrigin: true,
    logLevel: 'debug',
    pathRewrite: {
      '^/comments': '/api/comments'
    },
    headers: {
      'Cookie': '....'
    }
  }
}

```
```javascript 1.8
// historyfallback.js

module.exports={
  // 告诉这个插件 怎么样的文件要重定向 是css 和img js 文件则不要
  htmlAcceptHeaders:[
    'text/html','application/xhtml-xml'
  ],
  rewrites: [
    {
      from: /^\/([a-zA-Z0-9]+\/?)([a-zA-Z0-9]+)/,
      to: function (context) {
        return  '/' + context.match[1] + context.match[2] + '.html'
      }
    }
  ]
}

```
