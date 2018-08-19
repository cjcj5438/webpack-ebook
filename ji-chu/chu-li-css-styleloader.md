---
description: 引入/css modules /less.sass/提前css代码
---

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
                           insertInto:"#div",
                           singleton:true,
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

 css-loader主要的是让js怎么import一个css样式文件进来

