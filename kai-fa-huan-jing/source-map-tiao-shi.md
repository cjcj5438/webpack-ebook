# source map 调试

 js source map

* css sourec map
  * css-loader.option.sourcemap
  * less-loader.option.sourcemap
  * sass-loader.option.sourcemap

Devtool \(主要\)

webpack.SourceMapDevToolPlugin' 

webapck.EvakSourceMapDevToolPlugin

* Development
  * eval
  * eval-source-map
  * cheap-eval-source-map
  * cheap-modlue-eval-source-map
  * cheap-module-source-map
* prduction
  * source-map
  * hidden-source-map
  * nosource-source-map 

```text
module.exports={
   entry:{},
   output:{},
   plugins:[
      new webpack.HotModuleReplacementPlugin(),
      new webpack.NameMoudlesPlugin()
   ],
   modules:{},
   
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
      hotOnly:true ,/*不刷新浏览器*/
      devtool:'cheap-module-source-map'
   }
}

cssloader 加sourcemap : true 要放到options 里面的
singleton: true, // 使用一个style标签  这个要关掉
```

