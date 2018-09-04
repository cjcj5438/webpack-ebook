# 图片处理

1. css中引入的图片&gt; file-loader
2. 自动合成雪碧图&gt; postcss-sprites \([请移步](chu-li-css-styleloader.md)\)
3. 压缩图片&gt; img-loader
4. base64图片&gt; url-loader
5. html文件中静态资源引用替换需要通过html-loader。

 

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
                           limit:5000, //limit:8129,//小于limit限制的图片将转为base64嵌入引用位置
                           publicPath:"",
 							//fallback:'file-loader',//大于limit限制的将转交给指定的loader处理
                           outputPath:'dist/',
                           //使用相对路径
                           useRelativePath:true
                       }
                   },
                   {
                       loader:'img-loader',
                       options:{
                           pngquant:{
                               quality:80 //图片质量
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

也可以根据实际需求选择svg-url-loader,image-webpack-loader等其他插件。
## 雪碧图 ##
sprites雪碧图合成会在css-loader 中 postcss-loader 使用postcss-sprites配置。

在这里也讲下使用webpack-spritesmith插件来处理 雪碧图的位图
```
 new SpritesmithPlugin({
    //设置源icons,即icon的路径，必选项
    src: {
      cwd: __dirname + '/imgs/pngs',
      glob: '*.png' //正则匹配，照着填即可
    },
    //设置导出的sprite图及对应的样式文件，必选项
    target: {
      image: __dirname + '/build/imgs/sprite.png',
      css: __dirname + '/build/imgs/sprite.css' 
    },
    //设置sprite.png的引用格式，会自己加入sprite.css的头部
    apiOptions: {
      cssImageRef: './sprite.png' //cssImageRef为必选项
    },
    //配置spritesmith选项，非必选
    spritesmithOptions: {
      algorithm: 'top-down',//设置图标的排列方式
      padding: 4 //每张小图的补白,避免雪碧图中边界部分的bug
    }
  })
```
##  矢量图处理 ##
开发中常用的矢量图为svg格式，既可以使用inline-svg-loader进行资源嵌入，也可以使用svg-sprite-loader将矢量图资源合并为雪碧图，具体采用哪种方案，需要由项目的实际情况来判断。矢量图的合并原理与位图稍有不同，感兴趣的读者可以自行搜索。