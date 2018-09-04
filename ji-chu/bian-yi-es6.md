# 编译 ES6

npm install babel-loader@8.0.0-beta.0 @babel/core -D 语法编译


npm install --save-dev babel-cli babel-preset-env 
或者 npm install @babel/preset-env -S 
# 语法规范  #
添加babel转译的语法规范 
```javascript
 {
    test: /.js$/,
    use: {
        loader: 'babel-loader',
        options: {
            presets: [
                ['@babel/preset-env',
                    {
                        targets: {
                            browsers: ['&gt; 1%', 'last 2 versions']
                        }
                    }
                ]
            ]
        }
    }
}
```


```text
 rules:[ 
   { 
     test:/.js$/, 
     use:{ 
       loader:'babel-loader', 
       options:{ 
         presets:[
           persets:['env']
         ] 
        }
      } 
   } 
]
```
> 新版本的babel已经计划支持在package.json中设置browserslist参数来指定需要适配的使用环境，也就是说同一套针对使用环境的配置被剥离出来，而被postcss,babel,autoprefixer等工具共享使用。看下面的例子

```json
//.babelrc
{
  "presets":[
      ["@babel/preset-env",{
          "targets":{
              "browsers":["last 2 versions"]
          }
      }]
  ],
  "plugins":[
      "transform-runtime"
  ]
}
```

# 全局垫片 #  
```javascript
npm i babel-polyfill -s
use：
//ES Module
import 'babel-polyfill'
//或 CommonJs
require ('babel-polyfill')
```
当往往实际并不是这样的，我们将整个babel-polyfill引入则会有400多k的体积
解决：babel-polyfill是基于core-js和regenerator构建的，只需要在引用时指明即可，例如：
```
import 'core-js/modules/es6.array.from';
//Arrow Function  Array.from method
Array.from([1, 2, 3]).map((i) => {
    return i * i;
});
```
>babel-polyfill的实现方式如问题推演中所提到的那样，就是污染了全局环境，而且你可能已经意识到，这个工具，要么简单配置后代码量激增，要么按需引用配置繁琐。除非是在中型以上项目中有兼容低版本IE的需求，否则不建议使用。
# 局部垫片 #

```text
npm install babel-runtime babel-polyfill -save
npm install babel-plugin-transform-runtime -save-dev
```
简单地说，除了实例方法以外，其他的特性babel-runtime都会帮你打好补丁。使用时直接在plugins配置项中添加babel-plugin-transform-runtime即可。

**比较：**
*babel-polyfill*

简单粗暴，他会污染全局环境，比如在不支持Promise的浏览器会polyfill一个全局的Promise对象供调用；另外，不支持的实例方法也在对应的构造函数原型链上添加要polyfill的方法。


*babel-runtime*

不会污染全局环境，会在局部进行polyfill，另外不会转换一些实例方法，如'abc'.includes('a')，其中的includes方法就不会翻译。它一般结合babel-plugin-transform-runtime来使用。