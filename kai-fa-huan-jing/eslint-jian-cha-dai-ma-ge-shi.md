# Eslint检查代码格式

```text
npm i eslint eslint-loader eslint-plugin-htm eslint-friendly-formatter -D
```

eslint

* eslint-loader
  * options.failOnWarning 
  * options.failOnError
  * options.formatter
  * options.outputReport 输出代码报告

eslint-plugin-html 检查html 里面的js

eslint-friendly-formatter错误警告格式

.eslintrc.\* 或者是在 package.json 中的 eslintConfig里面写

配置eslint

* javascript Standard style \(http://standardjs.com\)遵循这里的js规范 
  * eslint-config-standard
  * eslint-plugin-promise
  * eslint-plugin-standard
  * eslint-plugin-import
  * eslint-plugin-node
  * eslint-config-xxx v





```text
rules:[
    {
        test:/\.js$/,
        //eslint检测的文件
        include:[path.resolve(__dirname, 'src')],
        //eslint不检测文件
        exclude:[path.resolve__dirname,'src/libs')]
        use:[
            {
                loader:'babel-loader',
                options:{
                    presets:[''env']
                }
            },
            {
                loader:'eslint-loader',
                options:{
                    formatter:require('eslint-friendly-formatter')
                }
            }
        ]
    }
]
```

添加规则文件

```text
add .esintrc.js
module.exports={
    root:true,
    extends:'standard',
    plugins:[
        //安装之前的html 插件
        'html'
    ],
    //定义环境 
    env:{
        brower:true,
        如果是node环境还可添加
        node:true
    },
    //可以自定义的覆盖上面的规则
    rulse:{},
    //把不允许的全局变量.变成允许,比如说是jq 的$
    globals:{
        $:true
    }
}
```

按插件

```text
npm install 
    eslint-config-standard 
    eslint-plugin-promise 
    eslint-plugin-standard 
    eslint-plugin-import 
    eslint-plugin-node 
-D
```

devServer.overlay   可以在浏览器中看到, 就可以用在控制台中看代码了

```text
devServer:{
      port:9001,
      overlay: ture    
}
```

