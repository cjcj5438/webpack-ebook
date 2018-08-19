# 打包公共代码

减少代码冗余

提高加载速度 CommonsChunkPlugin

```text
{
  plugins:[
    new webpack.optimize.CommonsChunkPlugin(option)
  ]
}
option可以配置的选项:
name 表示这个chunk 的名称
filename 表示公共代码打包的文件名
minChunks 数组函数,等. 出现代码出现的次数. 
chunks  提起代码的范围
children 是不是在模块中找依赖
deepChildren
async 异步的公共代码块
```

配置 场景 单页应用 单页应用+第三方依赖 单页应用+第三方依赖+webpack生产代码

实战

```text
var webpack=require('webpack')
var path=require('path')

module.exports={
    entry:{
        'pageA':'./src/pageA',
        'pageB':'./src/pageB',
        //*1** 这个时候想不业务代码和第三方代码分隔开怎么办
        'vendor':['lodash']
        
    },
    output:{
        path:path.resolve(__dirname,'./dist'),
        filename:'[name].bundle.js',//输出的文件
        chunkFilename:'[name].chunk.js'//输出的chunk公共文件
    },
   plugins:[
       new webpack.optimize.CommonsChunkPlugin({
            name :'common',
            minChunks:2,
            chunks:['pageA','pageB']
       }),
       /*
       new webpack.optimize.CommonsChunkPlugin({
            //*2** 这个时候把lodash和公共代码打到了一起
            name :'vendor',
            minChunks:Infinity
       }),
       new webpack.optimize.CommonsChunkPlugin({
            //*3** 把lodash分开  公共代码
            name :'chenjing',//换一个名字即可
            minChunks:Infinity
       }),
       */
       new webpack.optimize.CommonsChunkPlugin({
            //*4** 我们发现第 2 3 步骤是一样的, 我们合并起来写
            names :['vendor','chenjing'],
            minChunks:Infinity
       })
       
   ]
}
```

