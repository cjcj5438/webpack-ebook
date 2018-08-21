# 图片处理

1. css中引入的图片&gt; file-loader
2. 自动合成雪碧图&gt; postcss-sprites \([请移步](chu-li-css-styleloader.md)\)
3. 压缩图片&gt; img-loader
4. base64图片&gt; url-loader

 

```text
module.exports={
    entry:{
        'app':'./src/app.js'
    },
    output:{
        path:path.resolve(__dirname,'./dist'),
        filename:'[name].bundle.js',//输出的文件
    },
   modules:{ 
       rules:[
           {},
           {
               loader:'babel-loader',
               options:{
                    presets:['env'],
                    plugins:['lodash']
               }
           },
           {
               test:/\.(png|jpg|jpeg|gif)$/,
               use:[
               /*
                   {
                       loader: 'file-loader',
                       options:{
                           // 背景图配置
                           public    :       "     
                           outputPath:'dist/',
                           useRelativePath:true
                       }
                   },*/
                   //这个时候发现 url-loader 也可以实现file-loader
                   {
                       loader:'url-loader',
                       option:{
                           name:'[name][hash:5]-min.[ext]',
                           limit:5000,
                           publicPath:"",
                           outputPath:'dist/',
                           useRelativePath:true
                       }
                   },
                   {
                       loader:'img-loader',
                       options:{
                           pngquant:{
                               quality:80
                           }
                       }
                   }
               ]
           }
       ]
   },
   plugins:[
       new webpack.optimize.uglifyJsPlugin()
   ]
}
```

