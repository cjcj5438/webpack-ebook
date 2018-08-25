# 处理 CSS - style-loader

## style-loader

{% hint style="success" %}
npm i style-loader css-loader -D

import 'base.css'
{% endhint %}

### style-loader

{% hint style="info" %}
主要是在创建标签这一块 ,就是把css创建到文档html中
{% endhint %}

```javascript
var webpack=require('webpack')
var path=require('path')

module.exports={
    entry:{
        'app':'./src/app.js'
    },
    output:{
        path:path.resolve(__dirname,'./dist'),
        filename:'[name].bundle.js',//输出的文件
    },
   plugins:[],
   modules:{ 
       rules:[
           {
               test:/\.css$/,
               use:[
               //顺序是有必要的, 越后面越先加载
                   {loader:'style-loader'},
                   {loader:'css-loader'},
               ]
           }
       ]
   }
}
```

### style-loader/url 

{% hint style="warning" %}
 要配合 file-loader 一起使用
{% endhint %}

### style-loader/useable

```text
import base from './css/base.css'
import common from './css/common.css'

var flag=ture
setInterval(()=>{
    if(flag) base.unuse()
    else base.use()
    flag=!flag
},5000)
```

### options

* [ ] insertAt\(插入位置\)
* [ ] insertInto\(插入到dom\)
* [ ] sinleton 是否使用一个style标签
* [ ] transform\(转化,浏览器环境下,插入页面前\)

```text
rules:[
           {
               test:/\.css$/,
               use:[
               //顺序是有必要的, 越后面越先加载
                   {loader:'style-loader',
                       options:{
                           insertInto: '#app', // 可以指定加在哪个标签下
                            singleton: true, // 使用一个style标签
                           transform:'./css.transform.css'
                       }
                   },
                   {loader:'css-loader'},
               ]
           }
       ]
```

```text
// css.transform.js
module.exports=function(css){
    console.log(css)
    console.log(window.innerWidth)
    if(windos.innerWidth>=768){
        return css.replace('red','green')
    }else{
        return css.replace('red','orange')
    }
}
```

## css-loader

{% hint style="success" %}
 css-loader主要的是让js怎么import一个css样式文件进来
{% endhint %}

### options

1. [ ] alias 解析别名 
2. [ ] importLoader\(@import\) 
3. [ ] Minimize 是否压缩 
4. [ ] modules 启用css-mudules
   * [ ] :local 从本地
   * [ ] :global
   * [ ] compose 继承某个样式
   * [ ] compose  .... from  path 从某个问价映入某个样式

```text
rules:[
           {
               test:/\.css$/,
               use:[
               //顺序是有必要的, 越后面越先加载
                   {loader:'style-loader'},
                   {loader:'css-loader',
                       options:{
                           //minmize:true,
                           modules:true,
                           localIdentName:
                           '[path][name]-[local]-[hash:base64:5]'
-                      }
                   },
               ]
           }
       ]
```

```text
场景一
.box{
    height:200px,
    width:200px,
    /*集成common.css 文件下的 .bigBox样式*/
    composes:bigBox from './common.css'
}
```

```text
场景二
import base from './css/base.css'
import common from './css/common.css'

var app=docoment.getElementById('xxx')
app.innerHTML=`<div class=${base.box}></div>`
```

## Less sass

```text
npm i less less-loader sass-loader node-sass -D
```

```text
modules:{ 
       rules:[
           {
               test:/\.less$/,
               use:[
               //顺序是有必要的, 越后面越先加载
                   {loader:'style-loader'},
                   {loader:'css-loader'},
                   {loader:'less-loader'}
               ]
           },
           
       ]
   }
```

## 提取CSS

```text
npm i extract-text-webpack-plugin webpack -D
```

```text
modules:{ 
       rules:[
           {
               test:/\.less$/,
               use:ExtractTextWebpackPlugin.extract({
               /*
               fallback当页面不提取的是用什么loader加载到页面中
               use 继续处理的loader
               要配其他option 可以添加 
               */
                   fallback:{loader:'style-loader'},
                   use:[
                       {loader:'css-loader'},
                       {loader:'less-loader'}
                   ]
               })
           },
           
       ]
   }
plugins:[
    new ExtractTextWebpackPlugin({
        //提取出来的css文件名
        //提取出来文件不能自动插入html中.后面会补充解决方法,
        filename:'[name].min.css',
        //指定css提取的范围,默认是false,只提取初始化的,不会提取异步的
        allChunk:false
    })
]
```

## PostCss

```text
npm i                 -D
        postcss
        postcss-loader 
        autoprefixer        帮助添加浏览器前缀
        postcss-cssnano     帮助css压缩,css-loader就是用这个的
        postcss-cssnext     使用css新语法,让浏览器识别
```

```text
modules:{ 
       rules:[
           {
               test:/\.less$/,
               use:ExtractTextWebpackPlugin.extract({
                   fallback:{loader:'style-loader'},
                   use:[
                       {loader:'css-loader'},
                       {
                           loader:'postcss-loader',
                           options:[
                               ident:'postcss',
                               plugins:[
                                   //require('autoprefixer')()
                                   require('postcss-sprites')({
                                       spritePath:'dist/assets/imgs/sprites',
                                       //处理苹果设备上的高清图 
                                       retina:true
                                   }),
                                   require('postcss-cssnext')()
                               ]
                           ]
                       }
                       {loader:'less-loader'}
                   ]
               })
           },
           
       ]
   },
plugins:[
    new ExtractTextWebpackPlugin({
        filename:'[name].min.css',
        allChunk:false
    })
]
```

>

### 优化: Browerslist

{% hint style="success" %}
所有插件都公用

* package.json
* .browerslist
{% endhint %}

### 其他插件

postcss-import

post-url

postcss-assets



