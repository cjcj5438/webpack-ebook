---
description: webpack2.0引进的新功能
---

# Tree-shaking

## JS.Tree-shaking

```text
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
   modules:{ },
   plugins:[
       new webpack.optimize.uglifyJsPlugin()
   ]
}
```

```text
/*案例我们用lodash来测试 会发现使用了uglifyJsPlugin还是会有70多kb的依赖
这时我们可以使用
npm i babel-plugin-lodash -D
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
           {},
           {
               loader:'babel-loader',
               options:{
                    presets:['env'],
                    plugins:['lodash']
               }
           }
       ]
   },
   plugins:[
       new webpack.optimize.uglifyJsPlugin()
   ]
}
//思考uglifyJsPlugin不是万能的.要结合实际打包结果要自己找解决方案
```

## CSS.TreeShaking

```text
npm i purifycss-webpack glob-all -D
```

```text
var PurifyCSS=require('purifycss-webpack')
var glob=require('glob-all')

 plugins:[
  new PurifyCSS({
    paths:glob.sync({
        path.resolve(__dirname,'./*.html'),
        path.resolve(__dirname,'./*.js'),
     })
   }),
 
   new webpack.optimize.uglifyJsPlugin()
 ]
```



