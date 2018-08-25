# html in webpack

## 生成 html

* htmlWebpackPlugin
  * options
    * template 模板文件
    * filename 指定文件,输出文件名
    * minify 是否压缩
    * chunks 指定那几个chunk 要加入到页面中来的
    * inject 帮助插入js css 文件

```text
npm i html-webpack-plugin -D
```

```text
先require 进来

plugins:[
      new HtmlWebpackPlugin({
            filename:'index.html',
            template:'./index.html',
            //inject:false
            chunks:['app'],//指定页面的entry中的 app入口.插入相关的chunk
            minify:{
                  collapseWhitespace:true //去空格
            }
      })
   ]
   //如果出现路径出去. 要到output上面找问题
```

## Html中引入图片

* html-loader
  * options
    * attrs:\[img:src\]

```text
比如我们在页面中插入了img标签的图片.

modules:{ 
       rules:[
              {
                     test:/\.html$/,
                     use:[
                            {
                                   loader:'html-loader',
                                   options:{
                                          attrs:['img:src','img:data-src']c
                                   }
                            }
                     ]
              }
       ]
},
这里思考一个问题. css和html的图片引用都统一的话, 那么就会有一方是找不到图片的.那么怎么解决呢?
1,配置 publicPath : '/'
2,<img src="${require('./src/assets/imgs/xxx.jpg')}">
```

## 配合优化

提前载入webpack 加载代码

inline-manifest-webpack-plugin\(有点bug\)

html-webpack-inline-chunk-plugin\(这里讲解这分插件\)

```text
const HtmlWebpackInlineChunkPlugin=require('html-webpack-inline-chunk-plugin')


...
modules:{ 
       rules:[
              {
                     test:/\.js$/,
                     use:[
                            {
                                   loader:'babel-loader',
                                   options:{
                                   //会自动转换那些环境不支持的代码。
                                          presets:['env']
                                   }
                            }
                     ]
              }
       ]
},
plugins:[
       new HtmlWebpackInlineChunkPlugin({
              inlineChunks:['manifest']
       })
       new webpack.optimize.CommonsChunkPlugin({
              //自己生产的公共代码取名为manifest
              name:'manifest'
       })
   ]
   
   //使用的时候要注意不要和Htmlwebpackplugin的chunk  起冲突
```



