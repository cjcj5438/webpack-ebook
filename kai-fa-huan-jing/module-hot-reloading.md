# module hot reloading

保持应用数据状态  
节省调试时间  
样式调试更快

deServer.hot

webpack.HotModuleReplacementPlugin 

webpack.NamedModulesPlugin 可以看到模块的相对路径

module.hot

module.hot.accept

module.hot.decline

```text
npm i webpack-dev-server -D

module.exports={
   entry:{},
   output:{},
   plugins:[
      new webpack.HotModuleReplacementPlugin(),
      new webpack.NameMoudlesPlugin()
   ],
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
      },
      hot:true,
      hotOnly:true /*不刷新浏览器*/
   }
}
```

```text
//当js 不变的时候. 通过这个自定义配置
if(module.hot){
    module.hot.accept()
}
```

```text
// 当是组建的时候

  if(module.hot){
    module.hot.accept('./component/a',function(){
      app.removeChild(list)
     let componentA = require ('./component/a').componentA
     let newList=componentA()
     app.appenChild(newList)
      list=newList
    })
}
```



