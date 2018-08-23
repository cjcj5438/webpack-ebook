---
description: 代理远程接口请求
---

# Proxy

* http-proxy-middleware 在设置参数的时候,可以参照这个库开参照
  * options
    * target 指定代理的地址
    * changeOrigin 改变url , 在虚拟上使用过
    * headers 增加请求抱头
    * logLevel 帮助调试的, 在命令行中显示
    * pathRewrite 重定向接口请求

```text

$.get('https://m.weibo.cn/api/comments/show',{
    id:'4193566666',
    page:1
},function(data){
    console.log(data)
})
这样访问肯定是会跨域的, 那么我们可以用webpack-dev-server的node服务去请求数据, proxy:{...} 
```

```text
那我们来实现
$.get('/api/comments/show',{
    id:'4193566666',
    page:1
},function(data){
    console.log(data)
})
```

```text
npm i webpack-dev-server -D

module.exports={
   entry:{},
   output:{},
   plugins:[],
   modules:{},
   devServer:{
      port:9001,
      historyApiFallback:{
         rewrites:[
            {
               from:/^\([a-zA-Z0-9]+\/?)([a-zA-Z0-9]+)/,
               to:function(context){
                  retrun '/'+context.match[1]+  context.match[2]+'.html'
               }
            }
         ]
      },
      proxy:{
         '/':{
            target:'https://m.weibo.cn',
            changeOrigin:true,
            logLevel :'debug',
             pathRewrite :{
                '^/comments':'/api/comments'
             },
             headers:{
                'Cookie':'....'
             }
         }
      }
   }
}
```



